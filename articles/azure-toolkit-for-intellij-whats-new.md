---
title: "Co je nového v sadě Azure nástrojů pro IntelliJ | Microsoft Docs"
description: "Další informace o nejnovější funkce v sadě nástrojů Azure pro IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 46ed791f-df59-416a-809e-f52345ad973c
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 23714856f0f778be04cf321e0726b53ade430f57
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Co je nového v sadě Azure nástrojů pro IntelliJ
## <a name="azure-toolkit-for-intellij-releases"></a>Azure nástrojů pro IntelliJ verze
Tento článek obsahuje informace o různých verzích a nejnovější aktualizace nástrojů Azure pro IntelliJ.

> [!NOTE]
> Je také Azure nástrojů pro integrovaného vývojového prostředí Eclipse. Další informace najdete v tématu [nástrojů Azure pro Eclipse].
> 
> 

### <a name="april-14-2017"></a>14. dubna 2017
Sady nástrojů Azure pro IntelliJ - dubna 2017 verze obsahuje následující vylepšení:

* **Vylepšené přihlašovací prostředí Azure**: Sada nástrojů Azure pro IntelliJ teď podporuje dvě metody protokolování do účtu Azure: *interaktivní* a *automatizovaná*. Další informace najdete v tématu [Azure přihlášení v pokyny pro Azure nástrojů pro IntelliJ].
* **Publikování pomocí Docker kontejnery**: můžete teď publikování webových aplikací jako kontejnery Docker pomocí sady nástrojů Azure pro IntelliJ. Další informace najdete v tématu [jak publikovat webovou aplikaci jako kontejner Docker pomocí sady nástrojů pro Azure pro IntelliJ].
* **Správa účtů úložiště**: Sada nástrojů Azure pro IntelliJ nyní podporuje správu účty úložiště ze zobrazení Průzkumníka Azure. Další informace najdete v tématu [Správa účtů úložiště pomocí Průzkumníka Azure pro IntelliJ].
* **Virtual Machine Management**: Sada nástrojů Azure pro IntelliJ teď podporuje správu virtuálních počítačů z okna Nástroj Průzkumník Azure. Další informace najdete v tématu [Správa virtuálních počítačů pomocí Průzkumníka Azure pro IntelliJ].
* **Odebrání vzdálené ladění podporu**. Vzdálené ladění webových aplikací Java v Azure App Service byl odebrán z nástrojů Azure pro IntelliJ; To je nezbytný k řešení některých problémů, které zákazníků měla zaznamenat při pomocí sady nástrojů.

### <a name="august-26-2016"></a>26 srpna 2016
Sada nástrojů Azure pro IntelliJ - srpna 2016 verze obsahuje následující vylepšení:

* **Vlastní JDK distribuce**. Sady nástrojů Azure pro IntelliJ nyní podporuje zadání a nasazení je libovolný JDK verze vaší webové aplikace Azure kontejneru:
  * Kromě JDKs poskytovaný platformou Azure můžete také z široký výběr zulština OpenJDK verze k dispozici v Azure Azul systémy.
  * Můžete také zadat distribuční JDK Pokud uložíte jako soubor ZIP do účtu úložiště.
* **Vylepšení Průzkumník Azure**:
  * Podpora pro správu virtuálního počítače pomocí nového modelu Resource Manager Azure: můžete zobrazit seznam, vytvářet a odstraňovat virtuální počítače založené na správci prostředků aniž byste museli opustit rozhraní IDE.
  * Podpora pro správu účtu úložiště blob pomocí Azure Resource Manager, které doplňuje stávající funkce pro správu "klasické" účty úložiště.
* **Ovladač Microsoft JDBC 6.0 pro SQL Server**. Tato aktualizace obsahuje nejnovější ovladač JDBC pro Microsoft SQL Server (verze 6.0), což je nyní zahrnuty jako knihovnu, kterou můžete snadno přidat do vašich projektů Java, a tím nahrazení starší verze.

### <a name="june-29-2016"></a>29. června 2016
Sada nástrojů Azure pro IntelliJ - června 2016 verze obsahuje následující vylepšení:

* **Požadavek Java 8**. Sada nástrojů Azure pro IntelliJ nyní vyžaduje Java 8, i když tento požadavek je pouze pro sady nástrojů – aplikace můžete nadále používat všechny verze jazyka Java, které jsou podporovány službou Azure.
* **Podporu pro nejnovější JDKs Java**. Nejnovější verze Java JDKs jsou nyní podporovány pomocí nástrojů Azure pro IntelliJ.
* **Podpora pro Azure SDK v2.9.1**. Nejnovější verzi sady Azure SDK je nyní minimální nezbytný předpoklad pro sady nástrojů Azure pro IntelliJ.
* **Integrované ukázky**. Sada nástrojů Azure pro IntelliJ teď nabízí několik ukázkových aplikací, což vývojářům začít pracovat.
* **Integrace nástroje HDInsight**. Nástroje HDInsight Azure jsou nyní dodávané společně s Azure Toolkit pro IntelliJ. Další informace najdete v tématu [modulu plug-in nástroje HDInsight pro IntelliJ].
* **Vzdálené ladění webových aplikací Java**. Sada nástrojů Azure pro IntelliJ teď podporuje vzdálené ladění webových aplikací Java v Azure App Service.

### <a name="april-12-2016"></a>12. dubna 2016
Sada nástrojů Azure pro IntelliJ - verze. dubna 2016 obsahuje následující vylepšení:

* **Podpora pro Azure SDK v2.9.0**. Nejnovější verzi sady Azure SDK je nyní minimální nezbytný předpoklad pro sady nástrojů Azure pro IntelliJ.
* **Různé použitelnost, odezvy a vylepšení výkonu související s webové aplikace Azure podporu**. Několik optimalizací výkonu v jak sadu nástrojů komunikuje s Azure výsledek v rychlejšího uživatelského rozhraní.
* **Možnost odstranit kontejner existující webové aplikace v Azure z v rámci IntelliJ**. Sada nástrojů Azure pro IntelliJ teď umožňuje odstranit existující kontejner webů Azure, aniž byste museli opustit IntelliJ.

## <a name="see-also"></a>Viz také
Další informace o sadách Azure Toolkit pro integrovaná vývojová prostředí pro Javu najdete na následujících odkazech:

* [nástrojů Azure pro Eclipse]
  * [Novinky v sadě Azure Toolkit pro Eclipse]
  * [Instalace sady Azure Toolkit pro Eclipse]
  * [Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]
* [Sada Azure Toolkit pro IntelliJ]
  * *Co je nového v sadě Azure nástrojů pro IntelliJ (v tomto článku)*
  * [Instalace sady Azure Toolkit pro IntelliJ]
  * [Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]
  * [Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]

Další informace o používání Javy v Azure najdete na webu [Středisko pro vývojáře Java].

<!-- URL List -->

[nástrojů Azure pro Eclipse]: ./azure-toolkit-for-eclipse.md
[Sada Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij.md
[Vytvoření webové aplikace Hello World pro Azure v prostředí Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Vytvoření webové aplikace Hello World pro systém Azure v rámci IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalace sady Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalace sady Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Pokyny k přihlášení pro sadu Azure Toolkit pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Novinky v sadě Azure Toolkit pro Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure přihlášení v pokyny pro Azure nástrojů pro IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[jak publikovat webovou aplikaci jako kontejner Docker pomocí sady nástrojů pro Azure pro IntelliJ]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[Správa účtů úložiště pomocí Průzkumníka Azure pro IntelliJ]: ./azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer.md
[Správa virtuálních počítačů pomocí Průzkumníka Azure pro IntelliJ]: ./azure-toolkit-for-intellij-managing-virtual-machines-using-azure-explorer.md

[Středisko pro vývojáře Java]: http://go.microsoft.com/fwlink/?LinkID=699547

[modulu plug-in nástroje HDInsight pro IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
