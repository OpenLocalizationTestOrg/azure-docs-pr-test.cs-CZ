---
title: "Začínáme s Azure AD v projektech Visual Studio WebApi | Microsoft Docs"
description: "Jak začít používat Azure Active Directory v projektech WebApi po připojení k nebo vytváření Azure AD pomocí sady Visual Studio připojené služby"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: a756316054dd3bb63f3b0a9f59621bb1345bc693
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Get Začínáme s Azure Active Directory a služby Visual Studio připojené (WebApi projekty)
> [!div class="op_single_selector"]
> * [Začínáme](vs-active-directory-webapi-getting-started.md)
> * [Co se přihodilo](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a>Vyžádání ověření řadičům přístup
Všechny řadiče ve vašem projektu byly ozdobené s **Autorizovat** atribut. Tento atribut vyžaduje, aby uživatel k ověření před přístupem k rozhraní API definované tyto řadiče. Chcete-li umožňují řadiči získat anonymní přístup, odeberte tento atribut z řadiče. Pokud chcete nastavit oprávnění na podrobnější úrovni, použijte atribut pro každou metodu, která vyžaduje ověření, namísto aplikace do třídy kontroleru.

## <a name="next-steps"></a>Další kroky
[Další informace o službě Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

