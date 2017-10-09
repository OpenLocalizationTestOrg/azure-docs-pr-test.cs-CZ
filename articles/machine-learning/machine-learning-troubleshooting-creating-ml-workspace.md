---
title: "Řešení potíží: Vytvořte a připojte tooa pracovní prostor Machine Learning | Microsoft Docs"
description: "Řešení pro běžné problémy při vytvoření a připojení tooan Azure Machine Learning prostoru"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 965a0025e85ba4e22c2b037edfa923e7f7599069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-create-and-connect-tooan-machine-learning-workspace"></a>Příručka pro řešení potíží: Vytvořte a připojte tooan pracovní prostor Machine Learning
Tato příručka poskytuje řešení pro některé nejčastější problémy při se nastavování pracovních prostorů Azure Machine Learning.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Vlastník pracovní prostor
tooopen pracovního prostoru v Machine Learning Studio, musíte být přihlášeni toohello Account Microsoft používá toocreate hello prostoru, nebo musí tooreceive Pozvánka z pracovního prostoru hello vlastníka toojoin hello. Z portálu Azure, které můžete spravovat hello hello pracovní prostor, který zahrnuje hello možnost tooconfigure přístup.

Další informace o pracovním prostoru Správa, najdete v části [spravovat pracovní prostor služby Azure Machine Learning].

[spravovat pracovní prostor služby Azure Machine Learning]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Povolených oblastí
Machine Learning je aktuálně k dispozici pro omezený počet oblastí. Pokud vaše předplatné neobsahuje jednu z těchto oblastí, mohou se zobrazit chybová zpráva hello, "V hello povolené oblasti používat žádná předplatná."

Přidat předplatné tooyour toorequest, který se v oblasti, vytvořit novou žádost o podporu společnosti Microsoft z hello portálu Azure, zvolte **fakturace** jako typ problému hello a postupujte podle kroků vyzve hello toosubmit vaši žádost.

## <a name="storage-account"></a>Účet úložiště
Hello služby Machine Learning musí dat toostore účet úložiště. Můžete použít existující účet úložiště, nebo můžete vytvořit nový účet úložiště, když vytvoříte hello nový pracovní prostor Machine Learning (Pokud máte kvóty toocreate nový účet úložiště).

Po vytvoření hello nový pracovní prostor Machine Learning je tooMachine Learning Studio přihlásit pomocí účtu Microsoft hello, které že jste použili toocreate hello prostoru. Pokud narazíte na hello chybová zpráva, "Prostoru nebyl nalezen" (podobně jako toohello následující snímek obrazovky), použijte následující kroky toodelete hello soubory cookie prohlížeče.

![Pracovní prostor nebyl nalezen][screen3]

**soubory cookie prohlížeče toodelete**

1. Pokud používáte Internet Explorer, klikněte na tlačítko hello **nástroje** tlačítka na hello pravém horním rohu a vyberte **Možnosti Internetu**.  

![Možnosti Internetu][screen4]

2. V části hello **Obecné** , klikněte na **odstranit...**

![Karta Obecné][screen5]

3. V hello **odstranit historii procházení** dialogové okno zkontrolujte, zda **soubory cookie a data webové stránky** je vybrána a klikněte na tlačítko **odstranit**.

![Odstraňte soubory cookie][screen6]

Po odstranění souborů cookie hello se restartujte hello prohlížeče a potom přejděte toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) stránky. Pokud budete vyzváni k zadání uživatelského jména a hesla, zadejte text hello stejný účet Microsoft, které že jste použili toocreate hello prostoru.

## <a name="comments"></a>Komentáře

Naším cílem je toomake hello Machine Learning prostředí jako bezproblémové nejblíže. Prosím post všechny komentáře a problémy v hello [fórum Azure Machine Learning](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toohelp nám mohla poskytovat lepší.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
