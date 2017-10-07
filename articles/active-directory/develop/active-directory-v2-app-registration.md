---
title: "aaaRegister aplikace s koncovým bodem v2.0 hello Azure AD pomocí portálu hello | Microsoft Docs"
description: "Jak tooregister aplikace se společností Microsoft pro povolení přihlášení a přístup k Microsoft služby pomocí koncového bodu v2.0 hello"
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: bb2f701f-3bc3-4759-94a5-8b9d53a8a0b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: c56c98906656062435516e820cb318a04c03149c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooregister-an-app-with-hello-v20-endpoint"></a>Jak tooregister aplikace s koncovým bodem v2.0 hello
toobuild aplikaci, která přijímá MSA & Azure AD přihlášení, je nutné nejdříve tooregister aplikace se společností Microsoft.  V tomto okamžiku nebude se moct toouse všechny existující aplikace, které jste uzavřeli s Azure AD nebo MSA – budete potřebovat toocreate brand nový.

> [!NOTE]
> Ne všechny scénáře Azure Active Directory a funkce jsou podporovány koncového bodu v2.0 hello.  toodetermine Pokud byste měli používat koncového bodu v2.0 hello, přečtěte si informace o [v2.0 omezení](active-directory-v2-limitations.md).
> 
> 

## <a name="visit-hello-microsoft-app-registration-portal"></a>Navštívit portál pro registraci aplikace Microsoft hello
Nejdůležitější - první návštěvě příliš[https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Toto je nový portál registrace aplikace, kde můžete spravovat aplikace Microsoft.

Přihlaste se pomocí buď osobní nebo pracovní nebo školní účet Microsoft.  Pokud nemáte buď, zaregistrujte si nový osobní účet. Pokračujte, nebude trvat dlouho – budete čekáme sem.

Provést? By měl nyní se díváte v seznamu aplikací Microsoftu, což je pravděpodobně prázdná.  Umožňuje změnit.

Klikněte na tlačítko **přidat aplikaci**a pojmenujte ho.  portál Hello přiřadí aplikace globálně jedinečné Id aplikace, které budete používat později ve svém kódu.  Pokud vaše aplikace obsahuje serverovou komponentu potřebného přístupových tokenů pro volání rozhraní API (vezměte v úvahu: Office, Azure nebo vlastní webové rozhraní API), budete muset toocreate **tajný klíč aplikace** i zde.

Dál přidejte hello platformy, které vaše aplikace bude používat.

* Pro webové aplikace, zadejte **identifikátor URI pro přesměrování** kde lze odesílat zprávy přihlášení.
* Pro mobilní aplikace přesměruje poznamenejte hello výchozí identifikátor uri automaticky vytvoří za vás.

Volitelně můžete přizpůsobit hello vzhled a chování stránky přihlášení v hello profilu.  Ujistěte se, že tooclick **Uložit** než budete pokračovat.

> [!NOTE]
> Při vytváření aplikace pomocí [https://apps.dev.microsoft.com/?deeplink=/appList](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), hello aplikace se zaregistruje v klientovi domácí hello hello účtu používáte toosign hello portálu.  To znamená, že nemůže zaregistrovat aplikaci v klientovi služby Azure AD pomocí osobního účtu Microsoft.  Nechcete-li explicitně tooregister aplikace v konkrétním klientovi, přihlaste se pomocí účtu, který původně vytvořil v něm.
> 
> 

## <a name="build-a-quick-start-app"></a>Sestavení aplikace rychlý start
Teď, když máte aplikaci Microsoft, můžete dokončit jeden z našich kurzů v2.0 rychlý start.  Zde je několik doporučení:

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

