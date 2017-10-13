---
title: "Potíže při přihlašování k aplikaci zákaznických | Microsoft Docs"
description: "Běžné rrors, které by mohlo být příčinou vám nebudou moci přihlásit k aplikaci jste si vytvořili s Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: b0df23e040a73d18968f547eef7347f14cc577c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a>Přihlášení k aplikaci zákaznických problémů

Existuje několik chyb, které by mohlo být příčinou vám nebudou moci přihlásit do aplikace. Největších důvod uživatelé setkávají tento problém je špatně nakonfigurovaný. aplikace.

## <a name="errors-related-to--misconfigured-apps"></a>Chyby související s nesprávně nakonfigurované aplikace

* Ověřte, že konfigurace na portálu shodovat, co máte ve vaší aplikaci. Konkrétně porovnejte ID klienta nebo aplikace, adresy URL odpovědí, tajné klíče nebo klíče klienta a identifikátor ID URI aplikace.

* Porovnání prostředků, kterou žádáte o přístup k v kódu s nakonfigurovanou oprávnění v **požadované prostředky** a zajistěte, aby vám pouze požadavky na prostředky, které jste nakonfigurovali.

* V tématu [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) všechny podobné chyby nebo problémy.

## <a name="next-steps"></a>Další kroky

[Příručka vývojáře pro službu Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[Souhlasu a integrace aplikací do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[Souhlasu a systému oprávnění rolích pro Azure AD v2.0 konvergované aplikace](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory>)
