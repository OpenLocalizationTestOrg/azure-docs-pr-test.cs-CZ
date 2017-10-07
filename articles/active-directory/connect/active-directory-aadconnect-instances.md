---
title: "Azure AD Connect: Instance služby synchronizace | Microsoft Docs"
description: "Tato stránka dokumenty zvláštní upozornění pro instancí služby Azure AD."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 2a0d8a599cf84cd6530bdbb24951156510d2cf3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Zvláštní upozornění pro instance
Azure AD Connect se nejčastěji používá s hello world wide instanci Azure AD a Office 365. Ale existují také další instance a ty mají různé požadavky na adresy URL a další důležité.

## <a name="microsoft-cloud-germany"></a>Microsoft Cloud Germany
Hello [Microsoft Cloud Německo](http://www.microsoft.de/cloud-deutschland) je svrchovaných Cloudová provozují zplnomocněnec němčině data.

| Adresy URL tooopen v proxy serveru |
| --- |
| \*. microsoftonline.de |
| \*.windows.net |
| + Seznamy odvolaných certifikátů |

Když se přihlásíte v klientovi Azure AD tooyour, musíte použít účet v doméně onmicrosoft.de hello.

Funkce aktuálně nejsou k dispozici v Německu cloudu Microsoft hello:

* **Azure AD Connect Health** není k dispozici.
* **Funkce Automatické aktualizace** není k dispozici.
* **Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.
* Nejsou k dispozici další služby Azure AD Premium.

## <a name="microsoft-azure-government-cloud"></a>Cloudu Microsoft Azure Government.
Hello [cloudu Microsoft Azure Government](https://azure.microsoft.com/features/gov/) je cloud pro US government.

Tento cloud byl podporován ve starších verzích nástroje DirSync. Ze sestavení 1.1.180 služby Azure AD Connect hello nové generace hello cloudu je podporována. Generování používá jen USA založené na koncových bodů a mít jiný seznam adres URL tooopen proxy serveru.

| Adresy URL tooopen v proxy serveru |
| --- |
| \*.microsoftonline.com |
| \*. microsoftonline.us |
| \*. gov.us.microsoftonline.com |
| + Seznamy odvolaných certifikátů |

Azure AD Connect není možné tooautomatically zjistit, zda váš klient Azure AD je umístěno v cloud vlády hello. Místo toho musíte tootake hello následující akce při instalaci Azure AD Connect.

1. Spuštění instalace služby Azure AD Connect hello.
2. Až uvidíte hello první stránka, kde je mají tooaccept hello smlouvy EULA, nebudete pokračovat, ale ponechte spuštění Průvodce instalací hello.
3. Spustit program regedit a změňte klíč registru hello `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` toohello hodnotu `2`.
4. Vraťte se Průvodce instalací toohello Azure AD Connect, přijměte hello smlouvy EULA a pokračovat. Během instalace, ujistěte se, zda text hello toouse **vlastní konfigurace** cesta instalace (a ne expresní instalaci). Pokračujte hello instalace jako obvykle.

Funkce aktuálně nejsou k dispozici v cloudu Microsoft Azure Government hello:

* **Azure AD Connect Health** není k dispozici.
* **Funkce Automatické aktualizace** není k dispozici.
* **Zpětný zápis hesla** je k dispozici pro verzi preview verzí Azure AD Connect 1.1.570.0 a po.
* Nejsou k dispozici další služby Azure AD Premium.

## <a name="next-steps"></a>Další kroky
Přečtěte si další informace o [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md).
