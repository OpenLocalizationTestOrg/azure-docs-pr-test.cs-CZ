---
title: "aaaHow toogrant oprávnění tooa zákaznických aplikace | Microsoft Docs"
description: "Jak toogrant oprávnění tooyour zákaznických aplikace pomocí hello parametr adresy URL nebo portálu Azure AD"
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
ms.openlocfilehash: e43a105fff60fbf912bdf4f60260f86ee289328d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogrant-permissions-tooa-custom-developed-application"></a>Jak toogrant oprávnění tooa zákaznických aplikace

Pokud chcete souhlas toogrant ho preventivně ve vaší aplikaci nebo jsou spuštěné došlo k chybě aplikace tooan nebyly dá souhlas, zkuste níže uvedeného postupu.

## <a name="how-tooperform-admin-consent-for-your-application"></a>Jak tooperform souhlas správce pro vaši aplikaci

Tato akce nemá vliv hello udělení souhlasu toohello aplikací pro všechny uživatele ve vaší organizaci.

1. Přejděte toohello **registrace aplikace** jako **globálního správce**, pak vyberte aplikace hello.

2. Vyberte **požadovaných oprávnění**a nakonec klikněte na tlačítko hello **udělit oprávnění** tlačítko hello horní části okna hello.

Alternativně můžete vytvořit žádost o příliš*login.microsoftonline.com* s konfigurací vaší aplikace a připojte na *& výzva = správce\_souhlas*. Po přihlášení pomocí přihlašovacích údajů správce, aplikace hello byl udělen souhlas pro všechny uživatele.

## <a name="how-tooforce-user-consent-for-your-application"></a>Jak tooforce souhlas uživatele pro vaši aplikaci

* Připojit do žádosti o ověření *& výzva = souhlasu* vyžadující koncoví uživatelé tooconsent pokaždé, když ověření.

## <a name="next-steps"></a>Další kroky

[TooAzureAD souhlasu a integrace aplikací](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[Souhlasu a systému oprávnění rolích pro AzureAD v2.0 konvergované aplikace](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
