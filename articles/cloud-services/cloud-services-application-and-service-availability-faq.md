---
title: "problémy s dostupností aaaApplication a služby pro Microsoft Azure Cloud Services – nejčastější dotazy | Microsoft Docs"
description: "V tomto článku jsou uvedené nejčastější dotazy o aplikaci a dostupnost služeb pro Microsoft Azure Cloud Services hello."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: cd0d9ba0beb1dc72d4761f8b89c2ecadb51c7e48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Aplikace a problémy s dostupností služby pro Azure Cloud Services: Časté otázky (FAQ)

Tento článek obsahuje nejčastější dotazy týkající se aplikace a problémy s dostupností služby pro [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Můžete také obrátit hello [cloudové služby virtuálních počítačů velikost stránky](cloud-services-sizes-specs.md) velikost informace.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Moje role tu recykluje. Se nějakou aktualizaci nasazuje pro moje Cloudová služba?
Přibližně jednou za měsíc, společnost Microsoft vydá nové verze hostovaného operačního systému pro virtuální počítače Windows Azure PaaS. Hostovaného operačního systému je jenom jedna taková aktualizace v kanálu hello. Verze může mít vliv mnoho dalších faktorů. Kromě toho Azure je spuštěná na stovky tisíc počítačů. Proto je možné toopredict hello přesné datum a čas, kdy se restartuje vaše role. Aktualizujeme s hello nejnovější informace, které jsme hello hostovaného operačního systému aktualizujte informačního kanálu RSS, ale byste měli zvážit, hlášena čas toobe přibližná hodnota. Jsme víte, že se jedná o problém pro zákazníky a pracují toolimit plán nebo přesněji čas restartování.

Kompletní informace o nejnovějších aktualizacích hostovaného operačního systému najdete v tématu [verze hostovaného operačního systému Azure a kompatibilních sad SDK](cloud-services-guestos-update-matrix.md).

Užitečné informace o restartování a ukazatele tootechnical podrobnosti aktualizace hosta a hostitelským operačním systémem, najdete v příspěvku blogu MSDN hello [restartuje kvůli Role Instance upgrady tooOS](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx).

## <a name="why-does-hello-first-request-toomy-cloud-service-after-hello-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Proč hello první požadavek toomy cloudové služby po hello služby bylo nečinné po určitý čas trvat déle než obvykle?
Jakmile hello Webový Server obdrží požadavek na první hello, nejprve znovu zkompiluje hello kód a potom zpracuje žádost o hello. Který je proč hello první požadavek trvá déle než hello ostatní. Ve výchozím nastavení získá hello fondu aplikací v případech nečinnosti uživatele vypnout. fond aplikací Hello bude taky recyklaci ve výchozím nastavení každých 1,740 minut (29 hodiny).

Fondy aplikací Internetové informační služby (IIS) může být pravidelně recykluje tooavoid nestabilním stavy, které můžou vést tooapplication havárie, zablokování nebo nevracení paměti.

Následující dokumenty Hello se vám pomůžou pochopit a zmírnění tohoto problému:
* [Oprava pomalé počáteční zatížení pro služby IIS](http://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [Aplikace webové služby IIS 7.5 po recyklaci fondu aplikací je velmi pomalé první požadavek.](http://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

Pokud chcete toochange hello výchozí chování služby IIS, budete potřebovat toouse spuštění úlohy, protože pokud použijete ručně instance webových rolí toohello změn, změny hello budou nakonec ztraceny.

Další informace najdete v tématu [jak tooconfigure a spusťte spuštění úlohy pro cloudové služby](cloud-services-startup-tasks.md).
