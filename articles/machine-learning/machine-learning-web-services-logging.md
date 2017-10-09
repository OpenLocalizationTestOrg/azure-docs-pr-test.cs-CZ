---
title: "aaaLogging webové služby Machine Learning | Microsoft Docs"
description: "Zjistěte, jak tooenable protokolování pro Machine Learning webové služby. Protokolování poskytuje další informace o toohelp řešení hello rozhraní API."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: raymondl;garye
ms.openlocfilehash: ed23933d52d2151af658af2307d7df8743071f65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a>Povolení protokolování webových služeb Machine Learning
Tento dokument obsahuje informace o protokolování schopností webových služeb Machine Learning hello. Protokolování poskytuje další informace, kromě právě číslo chyby a zprávu, která vám pomohou vyřešit váš toohello volání rozhraní API pro Machine Learning.  

## <a name="how-tooenable-logging-for-a-web-service"></a>Jak tooenable protokolování pro webové služby

Povolit protokolování z hello [webové služby Azure Machine Learning](https://services.azureml.net) portálu. 

1. Přihlaste se na portál webové služby Azure Machine Learning toohello [https://services.azureml.net](https://services.azureml.net). Pro webovou službu Classic, můžete také získat toohello portál kliknutím **nové webové služby prostředí** na stránku webové služby Machine Learning hello v nástroji Machine Learning Studio.

   ![Nový odkaz webové služby prostředí](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. V horní nabídce hello, klikněte na tlačítko **webové služby** pro nové webové služby, nebo klikněte na tlačítko **Classic webové služby** pro klasický webové služby.

   ![Vyberte nový nebo Classic webových služeb](media/machine-learning-web-services-logging/select-web-service.png)

3. Pro novou webovou službu klikněte na název hello webové služby. Pro webovou službu Classic klikněte na název hello webové služby a potom klikněte na další stránku hello hello příslušný koncový bod.

4. V horní nabídce hello, klikněte na tlačítko **konfigurace**.

5. Sada hello **povolit protokolování** možnost příliš*chyba* (pouze chyby toolog) nebo *všechny* (pro úplné protokolování).

   ![Vyberte úroveň protokolování](media/machine-learning-web-services-logging/enable-logging.png)

6. Klikněte na **Uložit**.

7. Webové služby Classic, vytvořit hello **ml diagnostiky** kontejneru.

   Všechny protokoly webové služby jsou uchovávány v kontejneru objektů blob s názvem **ml diagnostiky** v účtu úložiště hello přidruženého k hello webové služby. Pro nové webové služby se tento kontejner vytvoří hello prvním spuštění aplikace hello webové služby. Webové služby Classic je nutné toocreate hello kontejneru, pokud ještě neexistuje. 

   1. V hello [portál Azure](https://portal.azure.com), přejděte toohello účtu úložiště přidruženého k hello webové služby.

   2. V části **služby objektů Blob**, klikněte na tlačítko **kontejnery**.

   3. Pokud kontejner hello **ml diagnostiky** neexistuje, klikněte na tlačítko **+ kontejner**přidělte hello kontejneru hello název "ml – Diagnostika" a vyberte hello **přistupovat typu** jako "Blob". Klikněte na **OK**.

      ![Vyberte úroveň protokolování](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Pro webovou službu Classic má hello řídicího panelu webové služby v nástroji Machine Learning Studio také tooenable protokolování přepínače. Protože protokolování se teď spravují prostřednictvím portálu hello webové služby, je však třeba tooenable protokolování prostřednictvím hello portálu, jak je popsáno v tomto článku. Pokud jste povolili protokolování v sadě Studio, v hello webový portál služby, zakázání protokolování a znovu ji povolte.


## <a name="hello-effects-of-enabling-logging"></a>účinky Hello povolení protokolování
Pokud je povoleno protokolování, hello diagnostiky a chyb z koncový bod webové služby hello přihlášeni hello **ml diagnostiky** kontejneru objektů blob v účtu úložiště Azure hello propojené s prostoru hello uživatele. Tento kontejner obsahuje všechny hello diagnostické informace pro všechny hello koncových bodů webové služby pro všechny pracovní prostory hello přidruženého k tomuto účtu úložiště.

protokoly Hello lze zobrazit pomocí kteréhokoli hello několik nástrojů k dispozici tooexplore účet úložiště Azure. Hello nejjednodušší může být účet úložiště toohello toonavigate hello portálu Azure, klikněte na tlačítko **kontejnery**a potom klikněte na kontejner hello **ml diagnostiky**.  

## <a name="log-blob-detail-information"></a>Podrobné informace o protokolu objektů blob
Každý objekt blob v kontejneru hello obsahuje hello diagnostické informace pro právě jeden z hello následující akce:

* Spuštění metody hello Batch Execution  
* Spuštění metody hello požadavků a odpovědí  
* Inicializace kontejner požadavků a odpovědí

Název Hello každý objekt blob může mít předponu hello následující formulář: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Kde _protokolu zadejte_ je jedním z hello následující hodnoty:  

* Batch  
* skóre nebo požadavky  
* skóre/init  

