---
title: "Rozšíření aplikace – Azure AD B2C | Microsoft Docs"
description: "Obnovení hello b2c-rozšíření – aplikace"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: f0392e32-0771-473c-a799-81438ca2bcff
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/17/2017
ms.author: parja
ms.openlocfilehash: b36410c18314bd893dc669b49814fdcd77fae054
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-b2c-extensions-app"></a>Azure AD B2C: Rozšíření aplikace

Když je vytvořen adresář služby Azure AD B2C, aplikace volá `b2c-extensions-app. Do not modify. Used by AADB2C for storing user data.` se automaticky vytvoří uvnitř hello nový adresář. Pro tuto aplikaci, označují tooas hello **aplikace b2c rozšíření**, se zobrazí na *registrace aplikace*. Je používána hello služby Azure AD B2C toostore. informace o uživatelích a vlastní atributy. Pokud aplikace hello odstraněna, Azure AD B2C nebude fungovat správně a bude mít vliv na produkční prostředí.

> [!IMPORTANT]
> Neodstraňujte hello b2c-rozšíření – aplikace, pokud plánujete tooimmediately odstranění vašeho klienta. Pokud aplikace hello zůstane odstraněné déle než 30 dní, uživatel informace budou trvale ztratí.

## <a name="verifying-that-hello-extensions-app-is-present"></a>Ověření rozšíření aplikaci hello je k dispozici

tooverify, který hello aplikace b2c rozšíření je k dispozici:

1. Ve vašem klientu Azure AD B2C, klikněte na **další služby** v levé navigační nabídce hello.
1. Vyhledejte a otevřete **registrace aplikace**.
1. Vyhledejte aplikaci, která začíná **aplikace b2c rozšíření**

## <a name="recover-hello-extensions-app"></a>Obnovit hello rozšíření aplikace

Pokud jste omylem odstranili hello b2c-rozšíření – aplikace, máte toorecover 30 dní ho. Můžete obnovit pomocí hello rozhraní Graph API aplikace hello:

1. Procházet příliš[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/).
1. Přihlaste se v lokalitě toohello jako globálního správce adresáře Azure AD B2C hello, které chcete aplikaci odstranit hello toorestore pro.
1. Vystavení HTTP GET na adresu URL hello `https://graph.windows.net/{tenantName}.onmicrosoft.com/deletedApplications` s verze api-version = 1.6. Ujistěte se, že tooreplace `{tenantName}` s název vašeho klienta. Tato operace se zobrazí seznam všech hello aplikací, které byly odstraněny v rámci hello posledních 30 dnech.
1. Najít hello aplikace v seznamu hello, kde název hello začíná "aplikace b2c rozšíření a zkopírujte její `objectid` hodnotu vlastnosti.
1. Vydání požadavku HTTP POST na adresu URL hello `https://graph.windows.net/myorganization/deletedApplications/{OBJECTID}/restore`. Nahraďte hello `{OBJECTID}` část adresy URL hello s hello `objectid` hello v předchozím kroku. 

Teď byste měli být schopný příliš[aplikace hello obnovit](#verifying-that-the-extensions-app-is-present) v hello portálu Azure.

> [!NOTE]
> Aplikace lze obnovit pouze pokud byla odstraněna v rámci hello posledních 30 dnů. Pokud to bylo víc než 30 dní, data budou trvale ztratí. O další pomoc soubor lístek podpory.
