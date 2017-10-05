---
title: "Zřizovat virtuální počítače vědecké účely dat Azure jako servery poznámkového bloku IPython | Microsoft Docs"
description: "Nastavte si Data virtuálního počítače vědecké účely jako Server poznámkového bloku IPython s podpůrnými nástroje."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 95e1fa87-794a-4d03-80a4-af4f3f3ac31e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: bradsev
ms.openlocfilehash: db1ffb2a226a087ecea2ea6f560c6b803e33d8c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="provision-azure-data-science-virtual-machines-as-ipython-notebook-servers"></a>Zřízení Azure Data vědecké účely virtuální počítače jako servery IPython Poznámkový blok
Pokyny najdete zde, které popisují postup nastavení virtuálního počítače Azure a virtuální počítač Azure se službou SQL jako servery IPython Poznámkový blok. Virtuální počítač Windows nakonfigurovaný s Podpora nástrojů, jako je IPython Poznámkový blok, Azure Storage Explorer a AzCopy, jakož i jiné nástroje, které jsou užitečné pro projekty vědecké účely data. Azure Storage Explorer a AzCopy, například poskytují pohodlný způsoby odesílání dat do úložiště Azure z místního počítače nebo se stáhne do místního počítače z úložiště. 

Tato nabídka odkazů na témata, které popisují, jak nastavit různé vědě prostředí datových používané [tým datové vědy procesu (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Několik typů virtuálních počítačích Azure můžete zřídit a nakonfigurované k použití jako součást prostředí cloudové data vědecké účely. Rozhodnutí o které příchuť použít virtuálního počítače závisí na typu a objemu dat modelovat s machine learning a cílového místa pro tato data v cloudu. 

* Pokyny k na otázky, které vzít v úvahu při tomto rozhodnutí, najdete v části [plánování vašeho Azure Machine Learning Data vědecké účely prostředí](machine-learning-data-science-plan-your-environment.md). 
* Katalog některého z následujících scénářů se můžete setkat při provádění pokročilou analýzu, najdete v části [scénáře pro pokročilé analýzy proces a technologie v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)

Jsou k dispozici dvě sady pokynů:

* [Nastavení virtuálního počítače Azure jako server IPython Poznámkový blok pro pokročilou analýzu](machine-learning-data-science-setup-virtual-machine.md) ukazuje, jak zřídit virtuální počítač Azure s IPython Poznámkový blok a jiných nástrojů, lze provést vědecké zpracování dat pro případy, ve kterých můžete formu úložiště než SQL Azure použije k uložení dat.
* [Nastavení se virtuální počítač Azure SQL Server jako server IPython Poznámkový blok pro pokročilou analýzu](machine-learning-data-science-setup-sql-server-virtual-machine.md) ukazuje, jak zřízení virtuálního počítače serveru SQL Azure s IPython Poznámkový blok a dalších nástrojů, které lze provést vědecké zpracování dat pro případy, ve kterých můžete databázi SQL použije k uložení dat.

Jakmile zřízený a nakonfigurované, tyto virtuální počítače jsou připravené pro použití jako servery IPython Poznámkový blok pro zkoumání a zpracování dat a dalších úlohách, které je potřeba v kombinaci s Azure Machine Learning a proces pro vědecké účely Data Team (TDSP). Další kroky v procesu vědecké účely data jsou namapované v [TDSP studijní](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesunutí dat do systému SQL Server nebo HDInsight, zpracování a ukázkové ho existuje v rámci přípravy learning z dat s počítači Azure Učení.

> [!NOTE]
> Virtuální počítače Azure jsou cenově jako **platíte jenom pro co používáte**. Aby se zajistilo, že nejsou se fakturuje Pokud nepoužíváte virtuální počítač, musí být v **zastaveno (Deallocated)** stavu z [portálu Azure Classic](http://manage.windowsazure.com/). Podrobné pokyny, jak nebo zrušit přidělení virtuálního počítače, naleznete v části [vypnutí a zrušit přidělení virtuálního počítače, když není používán](machine-learning-data-science-setup-virtual-machine.md#shutdown)
> 
> 

