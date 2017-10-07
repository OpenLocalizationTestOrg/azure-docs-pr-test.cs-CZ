---
title: "Virtuální počítače Azure dat vědecké účely jako servery poznámkového bloku IPython aaaProvision | Microsoft Docs"
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
ms.openlocfilehash: 7c0abcbcb822918332f76a9f16690a72b90f4b3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="provision-azure-data-science-virtual-machines-as-ipython-notebook-servers"></a>Zřízení Azure Data vědecké účely virtuální počítače jako servery IPython Poznámkový blok
Pokyny jsou k dispozici zde popisují, jak tooset virtuální počítač Azure a virtuální počítač Azure se službou SQL jako servery IPython Poznámkový blok. virtuální počítač Windows Hello nastavena podpora nástrojů, jako je IPython Poznámkový blok, Azure Storage Explorer a AzCopy, jakož i jiné nástroje, které jsou užitečné pro projekty vědecké účely data. Azure Storage Explorer a AzCopy, například nabízejí praktické způsoby úložiště tooAzure tooupload data z místního počítače nebo toodownload ho tooyour místního počítače z úložiště. 

Tato nabídka odkazy tootopics, které popisují, jak tooset až hello různé vědě prostředí datových používají v hello [tým datové vědy procesu (TDSP)](data-science-process-overview.md).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Několik typů virtuálních počítačích Azure můžou být zřízení a nakonfigurování toobe používá jako součást prostředí cloudové data vědecké účely. Hello rozhodnutí o které příchuť virtuálního počítače toouse závisí na typu hello a objemu dat toobe modelovány pomocí strojového učení a hello cílového místa pro tato data v cloudu hello. 

* Pokyny k hello otázky tooconsider při tomto rozhodnutí, najdete v části [plánování vašeho Azure Machine Learning Data vědecké účely prostředí](machine-learning-data-science-plan-your-environment.md). 
* Katalog některé scénáře hello se můžete setkat při provádění pokročilou analýzu, najdete v části [scénáře pro hello pokročilé analýzy proces a technologie v Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)

Jsou k dispozici dvě sady pokynů:

* [Nastavení virtuálního počítače Azure jako server IPython Poznámkový blok pro pokročilou analýzu](machine-learning-data-science-setup-virtual-machine.md) ukazuje, jak tooprovision virtuální počítač Azure s IPython Poznámkový blok a dalších nástrojů používá toodo vědecké zpracování dat pro případy, kdy formu úložiště než SQL Azure lze použít toostore hello data.
* [Nastavení se virtuální počítač Azure SQL Server jako server IPython Poznámkový blok pro pokročilou analýzu](machine-learning-data-science-setup-sql-server-virtual-machine.md) ukazuje, jak tooprovision se virtuální počítač Azure SQL Server pomocí poznámkového bloku IPython a dalších nástrojů používá toodo vědecké zpracování dat pro případy, kdy databáze SQL lze použít toostore hello data.

Jakmile zřízený a nakonfigurované, tyto virtuální počítače jsou připravené pro použití jako servery IPython Poznámkový blok pro zkoumání hello a zpracování dat a dalších úlohách, které je potřeba v kombinaci s Azure Machine Learning a hello tým datové vědy procesu (TDSP). Hello další kroky v procesu vědecké účely hello data jsou v namapované hello [TDSP studijní](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a může obsahovat kroky, které přesunutí dat do systému SQL Server nebo HDInsight, zpracování a ukázkové ho existuje v rámci přípravy učení se z hello dat pomocí Azure Machine Learning.

> [!NOTE]
> Virtuální počítače Azure jsou cenově jako **platíte jenom pro co používáte**. tooensure, která nejsou se účtují, pokud nepoužíváte virtuální počítač, má toobe v hello **zastaveno (Deallocated)** stavu z hello [portálu Azure Classic](http://manage.windowsazure.com/). Podrobné pokyny nebo jak toodeallocate jste virtuální počítač, najdete v části [vypnutí a zrušit přidělení virtuálního počítače, když není používán](machine-learning-data-science-setup-virtual-machine.md#shutdown)
> 
> 

