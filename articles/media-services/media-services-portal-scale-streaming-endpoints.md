---
title: "aaaScale streamování koncové body pomocí hello portálu Azure | Microsoft Docs"
description: "Tento kurz vás provede kroky hello škálování koncových bodů streamování s hello portálu Azure."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 1008b3a3-2fa1-4146-85bd-2cf43cd1e00e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: e466edf9232558b9e270f54ee2849cd9b22ad121
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="scale-streaming-endpoints-with-hello-azure-portal"></a>Škálování koncových bodů streamování s hello portálu Azure
## <a name="overview"></a>Přehled

> [!NOTE]
> toocomplete tohoto kurzu potřebujete účet Azure. Podrobnosti najdete v článku [Bezplatná zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/). 
> 
> 

Koncové body streamování **Premium** jsou vhodné pro pokročilé úlohy a poskytují vyhrazenou a škálovatelnou kapacitu šířky pásma. Zákazníci, kteří mají koncový bod streamování **Premium**, ve výchozím nastavení získají jednu jednotku streamování (SU). koncový bod streamování Hello je možné rozšířit přidáním služby SUs. Každý SU poskytuje dodatečnou šířku pásma kapacity toohello aplikace. Další informace o datových proudů typy koncových bodů a konfigurace CDN najdete v tématu hello [koncový bod streamování přehled](media-services-portal-manage-streaming-endpoints.md) tématu.
 
Toto téma ukazuje, jak tooscale koncový bod streamování.

Podrobné informace o cenách najdete v článku [Ceny služby Azure Media Services](http://go.microsoft.com/fwlink/?LinkId=275107).

## <a name="scale-streaming-endpoints"></a>Škálování koncových bodů streamování

toochange hello počet jednotek streamování, hello následující:

1. V hello [portál Azure](https://portal.azure.com/), vyberte svůj účet Azure Media Services.
2. V hello **nastavení** vyberte **koncové body streamování**.
3. Klikněte na koncový bod streamování hello, že chcete tooscale. 

    [!NOTE] Je možné škálovat jenom **Premium** koncové body streamování.

4. Přesuňte hello posuvníku toospecify hello počet jednotek streamování.

    ![Koncový bod streamování](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints3.png)

## <a name="next-steps"></a>Další kroky
Prohlédněte si mapy kurzů k Media Services.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

