---
title: "Testovací soubor Sipi | Microsoft Docs"
description: "Testovací soubor ke kontrole ReadyForTest závislosti"
services: active-directory-b2c
documentationcenter: 
author: Sipi
manager: Sipi
editor: Sipi
ms.assetid: 20e92275-b25d-45dd-9090-181a60c99f68
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/13/2017
ms.author: Sipi
ms.openlocfilehash: 871d58818dcbaee5f7a5f07c19e2297ec6459a6f
ms.sourcegitcommit: b0af2a2cf44101a1b1ff41bd2ad795eaef29612a
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/28/2017
---
# <a name="sipi-test-file"></a>Testovací soubor Sipi

Tento rychlý start vám pomůže zaregistrovat aplikaci v tenantovi Microsoft Azure Active Directory (Azure AD) B2C během několika minut. Jakmile budete hotovi, vaše aplikace bude zaregistrovaná pro použití v tenantovi Azure B2C.

## <a name="prerequisites"></a>Požadavky

Chcete-li sestavit aplikaci, která podporuje registrace a přihlašování uživatelů, musíte aplikaci nejprve zaregistrovat pomocí klienta Azure Active Directory B2C. Vlastního klienta získáte pomocí návodu v tématu [Vytvoření klienta Azure AD B2C](active-directory-b2c-get-started.md).

Aplikace vytvořené z okna Azure AD B2C na webu Azure Portal se musí spravovat ze stejného místa. Pokud upravíte aplikace B2C pomocí PowerShellu nebo jiného portálu, stanou se nepodporované a nebudou s Azure AD B2C pracovat. Podrobnosti najdete v části věnující se [chybným aplikacím](#faulted-apps). 

## <a name="navigate-to-b2c-settings"></a>Přechod do nastavení B2C

Přihlaste se k webu [Azure Portal](https://portal.azure.com/) jako globální správce tenanta B2C. 

[!INCLUDE [active-directory-b2c-switch-b2c-tenant](../includes/active-directory-b2c-switch-b2c-tenant.md)]

Další postup zvolte podle typu aplikace, kterou registrujete:
