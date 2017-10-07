---
title: "aaaApp témata nápovědy portálu registrace | Microsoft Docs"
description: "Popis různých funkcí v portálu pro registraci aplikace Microsoft hello."
services: active-directory
documentationcenter: 
author: lnalepa
manager: mbaldwin
editor: 
ms.assetid: f0507c28-9464-4d3e-bd53-de9053fd5278
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/16/2016
ms.author: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 3eb17b629577446a336152799497e7d980fb825d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="app-registration-reference"></a>Informace o registraci aplikace
Tento dokument obsahuje kontextu a popisy různé funkce, které se nacházejí v portálu pro registraci aplikace Microsoft hello [https://apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).

## <a name="my-applications"></a>Moje aplikace
Tento seznam obsahuje všechny aplikace zaregistrovaný pro použití s koncovým bodem v2.0 hello Azure AD.  Tyto aplikace mít toosign hello možnost uživatelé s oba osobní účty z účtu Microsoft a pracovní nebo školní účty ze služby Azure Active Directory.  toolearn Další informace o koncového bodu v2.0 hello Azure AD, najdete v našich [v2.0 přehled](active-directory-appmodel-v2-overview.md).  Tyto aplikace může být také použít toointegrate s hello Microsoft účet koncový bod ověřování, `https://login.live.com`.

## <a name="live-sdk-applications"></a>Live SDK aplikace
Tento seznam obsahuje všechny aplikace zaregistrovaný pro použití výhradně pomocí účtu Microsoft.  Pro použití se službou Azure Active Directory jakkoli není povolen.  Toto je, kde najdete všechny aplikace, které byl dříve registrován u hello MSA portál pro vývojáře v `https://account.live.com/developers/applications`.  Všechny funkce, které jste provedli dříve v `https://account.live.com/developers/applications` provést nyní tento nový portál `https://apps.dev.microsoft.com`.  Pokud máte další dotazy o aplikace účtu Microsoft, kontaktujte nás.

## <a name="application-secrets"></a>Tajné klíče aplikace
Tajné klíče aplikace jsou přihlašovací údaje, které umožňují vaší aplikaci tooperform spolehlivé [ověření klienta](http://tools.ietf.org/html/rfc6749#section-2.3) s Azure AD.  V OAuth a OpenID Connect, tajné klíče aplikace se běžně označují tooas `client_secret`.  V protokolu v2.0 hello, všechny aplikace, která přijímá token zabezpečení v umístění adresovatelné webové (pomocí `https` schéma) musíte použít tajný tooidentify aplikace, samotné tooAzure AD po uplatnění tohoto tokenu zabezpečení.  Kromě toho všechny nativní klient systému, bude je zakázané recieves tokeny na zařízení pomocí ověření klienta aplikace tajný tooperform, toodiscourage hello úložiště tajných klíčů v nezabezpečené prostředí.

Každá aplikace může obsahovat dva tajné klíče platnou aplikaci v libovolném bodě v čase.  Udržovat dva tajné klíče, máte napříč celou prostředí aplikace hello ablilty tooperform pravidelné výměny klíčů.  Po přesunutí hello celého vaší aplikace tooa nový sdílený tajný klíč, můžete odstranit hello starý sdílený tajný klíč a zřízení nového.

V tomto okamžiku jsou povolené jenom dva typy tajné klíče aplikace v portálu pro registraci aplikace hello.  Výběr **generovat nové heslo** bude generovat a ukládat sdílený tajný klíč v úložišti hello příslušných dat, který můžete použít v aplikaci.  Výběr **vygenerovat nový pár klíč** vytvoří nový pár veřejného a privátního klíče, je možné stáhnout a použít pro klienta ověřování tooAzure AD.

## <a name="profile"></a>Profil
část profilu Hello portálu pro registraci aplikace hello může být používán toocustomize hello přihlašovací stránka pro vaši aplikaci.  V tuto chvíli můžete změnit hello přihlašovací stránky aplikace loga, podmínky adresa URL služby a zásady ochrany osobních údajů.  Hello logo musí být transparentní 48 × 48 nebo 50 × 50 pixelů v souboru GIF, PNG nebo JPEG, který je 15 KB nebo menší.  Zkuste změnit hodnoty hello a zobrazení hello výsledná přihlašovací stránky!

## <a name="live-sdk-support"></a>Live SDK podpory
Pokud povolíte "podporu SDK Live", aplikace, kterou tajné klíče, které vytvoříte, se zřídí do hello Azure AD a Account Microsoft data uloží.  To vám umožní toointegrate vaší aplikace přímo s hello služby Microsoft Account (login.live.com).  Pokud chcete toobuild aplikace pomocí Account Microsoft přímo (jako koncový bod v2.0 názvem na rozdíl od toousing hello Azure AD), měli byste si ověřit, že je zapnutá podpora Live SDK.

Zakázání podpory Live SDK zajistí, že je tento tajný klíč aplikace hello zapsat pouze do úložiště dat hello Azure AD.  úložiště dat Hello Azure AD zahrnuje předpisy podnikové úrovni, které umožňují ho toomeet nějaké standardy, jako je například FISMA dodržování předpisů.  Pokud povolíte podporu Live SDK, vaše aplikace nemusí zajištění dodržování předpisů pomocí některé z těchto standardů.

Pokud máte v plánu vždy jen koncového bodu v2.0 toouse hello Azure AD, můžete bezpečně zakázat podporu Live SDK.

