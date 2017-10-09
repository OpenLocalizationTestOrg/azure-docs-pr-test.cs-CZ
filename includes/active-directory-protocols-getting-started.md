---
title: "aaaAzure AD přehled protokolu .NET | Microsoft Docs"
description: "Jak toouse HTTP zprávy tooauthorize přistupovat k tooweb aplikací a webových rozhraní API ve vašem klientovi pomocí služby Azure AD."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
ms.openlocfilehash: 5bd54af028c445afd3f35d67d47d7c84b476c757
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
## Registrace aplikace pomocí tenanta AD
Nejprve budete potřebovat tooregister vaší aplikace pomocí klienta služby Azure Active Directory (Azure AD). To se poskytují ID aplikací pro aplikace, a také ji povolit tooreceive tokeny.

* Přihlaste se toohello [portálu Azure](https://portal.azure.com).
* Kliknutím na vašem účtu v hello pravém horním rohu stránky hello zvolte klientovi Azure AD.
* V levém navigačním podokně hello, klikněte na **Azure Active Directory**.
* Klikněte na **Registrace aplikací** a potom klikněte na **Přidat**.
* Postupujte podle pokynů hello a vytvořte novou aplikaci. Pro účely tohoto kurzu není důležité, jestli je to webová nebo nativní aplikace. Pokud ale chcete konkrétní příklady webových nebo nativních aplikací, podívejte se na naše [rychlé průvodce](../articles/active-directory/develop/active-directory-developers-guide.md).
  * Pro webové aplikace, zadejte hello **přihlašovací adresa URL** tedy hello základní adresu URL aplikace, kde uživatelé se mohou přihlásit v např `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: hello **App ID URI** is a unique identifier for your application. hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * U nativních aplikací, zadejte **identifikátor URI pro přesměrování**, které Azure AD budou používat tooreturn odpovědi tokenu. Zadejte hodnotu konkrétní tooyour aplikace pomocí. např`http://MyFirstAADApp`
* Po dokončení registrace Azure AD se přiřazení klienta jedinečný identifikátor vaší aplikace, hello ID aplikace. Je nutné tuto hodnotu v dalších částech hello, takže zkopírujte jej ze stránky aplikace hello.
