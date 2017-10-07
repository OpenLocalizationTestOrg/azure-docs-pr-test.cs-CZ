---
title: "na stránce hello aaaLinks nefungují pro aplikaci Proxy aplikace | Microsoft Docs"
description: "Jak tootroubleshoot problémy se poškozených odkazů na Proxy aplikace aplikací, které mají integrované s Azure AD"
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
ms.openlocfilehash: 77c1e22d27c7a6436d8e57e105037c2328180481
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="links-on-hello-page-dont-work-for-an-application-proxy-application"></a>Odkazy na stránce hello nefungují pro aplikaci Proxy aplikace

Tento článek vám pomohou tootroubleshoot proč odkazy na aplikace Proxy aplikace služby Active Directory Azure nefungují správně.

## <a name="overview"></a>Přehled 
Po publikování aplikace Proxy aplikace, že práci v aplikaci hello ve výchozím nastavení jsou toodestinations odkazy obsažené v hello pouze odkazy hello publikovat adresy URL kořenového adresáře. Hello odkazy v aplikacích hello nefungují, hello interní adresa URL pro aplikaci hello pravděpodobně nezahrnuje všechny cíle hello odkazů v rámci aplikace hello.

**Proč to stává?** Při kliknutí na odkaz v aplikaci, hello Proxy aplikace pokusů tooresolve hello adresu URL buď interní adresu URL uvnitř stejné aplikace, nebo jako adresu URL externě dostupný. Pokud hello odkaz odkazuje tooan interní URL, která není v rozsahu hello stejné aplikace, není patří tooeither těmito oblastmi a způsobit chybu nebyl nalezen.

## <a name="ways-you-can-resolve-broken-links"></a>Způsoby, můžete vyřešit nefunkční odkazy

Existují tři způsoby tooresolve tento problém. Hello volby níže jsou uvedeny v rostoucí složitostí.

1.  Ujistěte se, že interní adresa URL hello kořenovou, která obsahuje všechny relevantní odkazy hello aplikace hello. To umožňuje všechny odkazy toobe vyřešit jako obsah publikovaný v rámci hello stejná aplikace.

    Pokud změníte hello interní adresa URL, ale nechcete, aby toochange hello úvodní stránka pro uživatele, toohello URL domovskou stránku hello změnu dříve publikované interní adresa URL. To lze provést tak, že přejdete příliš "Azure Active Directory" -&gt; registrace aplikace -&gt; vyberte aplikace hello –&gt; vlastnosti. Na této kartě Vlastnosti zobrazí pole hello "Domovskou stránku URL", lze ji nastavit toobe hello potřeby cílová stránka.

2.  Pokud vaše aplikace použít plně kvalifikované názvy domény (FQDN), použijte [vlastní domény](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) toopublish vaší aplikace. Tato funkce umožňuje hello používá stejnou adresu URL toobe jak interně a externě.

    Tato možnost zajistí hello odkazy v aplikaci externě přístupné prostřednictvím Proxy aplikace od hello odkazy v rámci adresy URL toointernal hello aplikace jsou také rozpoznány externě. Všimněte si, že všechny odkazy stále nutné toobelong tooa publikované aplikace. Však se tato možnost hello odkazy nemusí toobelong toohello stejnou aplikaci a můžou patřit toomultiple aplikace.

3.  Pokud žádná z těchto možností jsou vhodná, připojíte hello preview pro novou funkci, která provádějí překlad/přepisování adresy URL. Tato možnost, interní adresy URL nebo odkazy, které existují v textu hello HTML aplikací mít přeložit, nebo "namapované", toohello publikovaná externí adresy URL Proxy aplikace. To funguje pouze pro odkazy v hello HTML nebo šablon stylů CSS, a to není pomoct v případě odkaz na vaši je generována prostřednictvím JS. 

V důsledku toho důrazně doporučujeme používat hello [vlastní domény](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) řešení, pokud je to možné. Pokud chcete toojoin hello preview, e-mailu < aadapfeedback@microsoft.com > s hello applicationId(s).

## <a name="next-steps"></a>Další kroky
[Práce s existující místní proxy servery](application-proxy-working-with-proxy-servers.md)

