---
title: "aaaHow tooget klient služby Azure AD | Microsoft Docs"
description: "Jak Azure Active Directory tooget klienta pro registraci a vytváření aplikací."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: 
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/19/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: dcc6b3109528cf763bda9bd527344ea9ab5c0d69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-an-azure-active-directory-tenant"></a>Jak Azure Active Directory tooget klienta
V Azure Active Directory (Azure AD) představuje [klient](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) zástupce organizace.  Je vyhrazenou instanci hello služby Azure AD, které organizace obdrží a vlastní, když se přihlásí ke cloudové službě Microsoftu, jako je například Azure, Microsoft Intune nebo Office 365.  Každý klient Azure AD se odlišuje a je oddělený od ostatních klientů Azure AD.  

V klientovi se nachází uživatelé hello ve společnosti a hello informace o nich – hesla, data uživatelského profilu, oprávnění a tak dále.  Obsahuje také skupiny, aplikace a další informace týkající se tooan organizace a jejího zabezpečení.

tooallow toosign uživatele Azure AD v aplikaci tooyour, je nutné zaregistrovat aplikaci v klientovi vlastní.  Publikování aplikace v klientovi Azure AD je **zcela zdarma**.  Většina vývojářů dokonce pro účely experimentování, vývoje, přípravy a testování vytváří několik klientů a aplikací.  Organizace, které si zaregistrují a používají vaši aplikaci můžete volitelně vybrat toopurchase licence, pokud si přejí tootake výhod pokročilých funkcí adresáře.

Jak tedy můžete získat klienta Azure AD?  Hello proces může být mírně lišit, pokud je:

* [Máte stávající předplatné služeb Office 365](#use-an-existing-office-365-subscription)
* [Máte stávající předplatné Azure spojené s účtem Microsoft](#use-an-msa-azure-subscription)
* [Máte stávající předplatné Azure spojené s účtem organizace](#use-an-organizational-azure-subscription)
* [Nemáte nic z výše uvedených hello & má toostart od začátku](#start-from-scratch)

## <a name="use-an-existing-office-365-subscription"></a>Použití stávajícího předplatného služeb Office 365
Pokud máte stávající předplatné Office 365, již tenanta služby Azure AD máte! Můžete přihlásit toohello [portál Azure](https://portal.azure.com) s vaší O365 účet a začít používat Azure AD.

## <a name="use-an-msa-azure-subscription"></a>Použití předplatného MSA Azure
Pokud jste si již dříve zaregistrovali předplatné služby Azure pomocí svého individuálního účtu Microsoft, již klienta máte!  Při přihlášení toohello [portálu Azure](https://portal.azure.com), budete automaticky přihlášeni v tooyour výchozího klienta. Jsou volné toouse najdete v části tohoto klienta podle potřeb – ale může být vhodné toocreate účet správce organizace.

toodo, takže podle těchto kroků.  Alternativně můžete chcete toocreate nového klienta a vytvořit správce v něm podle podobného postupu.

1. Přihlaste se k hello [portálu Azure](https://portal.azure.com) pomocí individuálního účtu
2. Přejděte v části "Azure Active Directory" toohello hello portálu (nalezen hello levém navigačního panelu v části **více služeb**)
3. Můžete by měl být automaticky přihlášeni v toohello "Výchozí adresář", pokud nejsou adresáře můžete přepnout kliknutím na název účtu v pravém horním rohu hello.
4. Z hello **Rychlé úlohy** zvolte **přidat uživatele**.
5. V hello formuláře pro přidání uživatele, zadejte následující podrobnosti hello:

   * Název: (zvolte příslušnou hodnotu)
   * Uživatelské jméno: (zvolte uživatelské jméno pro tohoto správce.)
   * Profil: (zadejte příslušné hodnoty hello křestní jméno, poslední název, funkce a oddělení)
   * Role: globální správce
6. Po dokončení hello formuláře pro přidání uživatele a přijímat hello dočasné heslo pro nového správce hello, že toorecord se toto heslo, budete je potřebovat toologin tohoto nového uživatele v pořadí toochange hello heslo. Můžete také odeslat heslo hello přímo toohello uživatele, pomocí alternativní e-mailu.
7. Klikněte na **vytvořit** toocreate hello nového uživatele.
8. toochange hello dočasné heslo, přihlaste se k [https://login.microsoftonline.com](https://login.microsoftonline.com) tohoto nového uživatele účtu a změnit heslo hello vyžádání.

## <a name="use-an-organizational-azure-subscription"></a>Použití organizačního předplatného Azure
Pokud jste si již dříve zaregistrovali předplatné služby Azure pomocí účtu organizace, již klienta máte!  V hello [portálu Azure](https://portal.azure.com), když přejdete příliš měli klienta najít "Více služby" a "Azure Active Directory."  Jste volné toouse, které vložit jak můžete vidět, tohoto klienta.

## <a name="start-from-scratch"></a>Začátek od nuly
Pokud všechny výše uvedené hello tooyou nesrozumitelné, nemusíte si dělat starosti.  Navštivte stránku [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) toosign pro službu Azure s novou organizací.  Po dokončení procesu hello, budete mít svého vlastního klienta Azure AD s názvem domény hello, které jste zvolili při registraci.  V hello [portálu Azure](https://portal.azure.com), vašeho klienta můžete najít tak, že přejdete příliš "Azure Active Directory" v hello vlevo.

Jako součást procesu hello registrace do Azure bude požadované tooprovide platební karty podrobnosti.  Můžete bez obav pokračovat – za publikování aplikací v Azure AD ani vytváření nových klientů vám nebude nic účtováno.
