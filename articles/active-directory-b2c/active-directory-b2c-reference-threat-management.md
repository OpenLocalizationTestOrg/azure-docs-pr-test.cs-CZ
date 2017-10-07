---
title: "Azure Active Directory B2C: Hrozby správy | Microsoft Docs"
description: "Další informace o zjišťování a ke zmírnění techniky pro útoky DOS a útoky heslo v Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: vigunase
manager: Ajith Alexander
editor: 
ms.assetid: 6df79878-65cb-4dfc-98bb-2b328055bc2e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2016
ms.author: 
ms.openlocfilehash: 60bc0cc392b332cc4e9741ddb97dfa58e68ed420
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-threat-management"></a>Azure Active Directory B2C: Hrozby správy

Správa hrozeb zahrnuje plánování pro ochranu před útoky na systém a sítě. Útoky DOS může aby prostředky k dispozici toointended uživatele. Heslo útoky realizace toounauthorized přístup tooresources. Azure Active Directory B2C (Azure AD B2C) obsahuje integrované funkce, které vám umožní chránit vaše data před tyto hrozby několika způsoby.

## <a name="denial-of-service-attacks"></a>Útoky DoS

Azure AD B2C používá detekce a zmírnění techniky, jako jsou soubory cookie SYN a rychlost a připojení tooprotect omezení, které jsou základní prostředky před útoky DOS.

## <a name="password-attacks"></a>Útoky heslo

Zmírnění techniky Azure AD B2C má také nastavené pro útoky heslo. Zmírnění dopadů zahrnuje útoky hrubou silou heslo a slovníkovým útokům heslo. Hesla, které jsou nastavené uživatelé jsou požadované toobe přiměřeně komplexní. Pomocí různých signály analyzuje Azure AD B2C hello integritu požadavků. Azure AD B2C je navržený tak, toointelligently rozlišit před hackery a botnetů příslušným uživatelům. Azure AD B2C poskytuje sofistikované strategie toolock účty podle hello hesel zadali v hello pravděpodobnost útoku.

Další informace najdete v článku hello [Microsoft Trust Center](https://www.microsoft.com/trustcenter/security/threatmanagement).
