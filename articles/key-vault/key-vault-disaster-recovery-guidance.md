---
title: "Co dělat v případě Azure služby přerušení, která má vliv na Azure Key Vault | Microsoft Docs"
description: "Zjistěte, co dělat v případě přerušení služby Azure, který má vliv na Azure Key Vault."
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
ms.openlocfilehash: 6419d54c54e7d19103419262b79e7a5268b2268c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault dostupnost a redundance
Azure Key Vault funkce více vrstev redundance a ujistěte se, že klíče a tajné klíče zůstanou k dispozici pro vaše aplikace i v případě jednotlivé komponenty služby selžou.

Obsah trezoru klíčů se replikují do oblasti a do sekundární oblasti aspoň 150 miles ji okamžitě, ale v rámci stejné geography. Tím se zachová vysokou odolnost klíčů a tajných klíčů. Najdete v článku [Azure spárovat oblasti](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions) dokumentu podrobnosti o konkrétní oblasti páry.

Pokud jednotlivých součástí v rámci trezoru klíčů služby selže, krok alternativní součásti v rámci oblasti v poskytovat vaši žádost a ujistěte se, že neexistuje žádná snížení funkčnosti. Není nutné provádět žádnou akci k aktivaci to. To se stane automaticky a budou průhledné vám.

V výjimečná událost, že není k dispozici celý oblasti Azure, se automaticky směrují požadavky, které budete z Azure Key Vault v daném regionu (*převzetí služeb při selhání*) do sekundární oblasti. Když primární oblasti opět k dispozici, požadavky na směrují zpět (*zpět se nezdařilo*) primární oblasti. Znovu není nutné provádět žádnou akci, protože to probíhá automaticky.

Existuje několik aspektů zajímat:

* V případě selhání oblast ho může trvat několik minut, než služba převzetí služeb při selhání. Odesílat požadavky provedené během této doby může selhat, dokud se nedokončí převzetí služeb při selhání.
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

