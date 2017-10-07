---
title: "aaaWhat toodo v události hello narušení služby Azure, který má vliv na Azure Key Vault | Microsoft Docs"
description: "Zjistěte, jaké toodo v události hello narušení služby Azure, který má vliv na Azure Key Vault."
services: key-vault
documentationcenter: 
author: adamglick
manager: mbaldwin
editor: 
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: sumedhb;aglick
ms.openlocfilehash: 88eec82ada401a28323b3eea126168185ba4cdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault dostupnost a redundance
Azure Key Vault funkce více vrstev redundance toomake klíče a tajné klíče zůstat k dispozici tooyour aplikace i v případě, že jednotlivé součásti aplikace hello služby nezdaří.

Hello obsah trezoru klíčů jsou replikované v rámci oblasti hello a sekundární oblasti tooa aspoň 150 miles ji okamžitě, ale v rámci hello stejné geography. Tím se zachová vysokou odolnost klíčů a tajných klíčů. Najdete v části hello [Azure spárovat oblasti](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentu podrobnosti o páry určité oblasti.

V případě jednotlivých součástí v rámci hello trezor klíčů služby selžou, alternativní součásti v rámci oblasti hello krok v tooserve vaše žádost o toomake se, že žádné snížení funkčnosti. Není nutné tootake všechny akce tootrigger to. To se stane automaticky a bude průhledná tooyou.

V hello výjimečná událost, není k dispozici celý oblasti Azure, se automaticky směrují hello požadavků, které provedete z Azure Key Vault v daném regionu (*převzetí služeb při selhání*) tooa sekundární oblast. Když primární oblasti hello opět k dispozici, požadavky na směrují zpět (*zpět se nezdařilo*) toohello primární oblasti. Znovu není nutné tootake žádnou akci vzhledem k tomu, že k tomu dojde automaticky.

Existují vědět několik toobe upozornění:

* V události hello selhání oblast se může trvat několik minut, než hello služby toofail. Odesílat požadavky provedené během této doby může selhat, dokud se nedokončí hello převzetí služeb při selhání.
* Po dokončení převzetí služeb trezoru klíčů je v režimu jen pro čtení. Požadavky, které jsou podporovány v tomto režimu jsou:
  * Seznam trezorů klíčů
  * Získat vlastnosti trezorů klíčů
  * Výpis tajných klíčů
  * Získat tajné klíče
  * Seznam klíčů
  * Získání klíčů (Vlastnosti)
  * Šifrování
  * Dešifrování
  * Zabalení
  * Rozbalení
  * Ověřit
  * Přihlášení
  * Zálohování
* Po převzetí služeb se nezdařilo zpět, všechny typy požadavku (včetně čtení *a* požadavků na zápis) jsou k dispozici.

