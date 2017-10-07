---
title: "Azure AD Connect: Výrazů deklarativního zřizování | Microsoft Docs"
description: "Vysvětluje výrazů deklarativního zřizování hello."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: e3ea53c8-3801-4acf-a297-0fb9bb1bf11d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 516bcf1991c608d33aefc19551254d8b2bfc024f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Synchronizace Azure AD Connect: Principy výrazů deklarativní zřizování
Synchronizace Azure AD Connect je založený na deklarativní zřizování poprvé dostupné ve verzi produktu Forefront Identity Manager 2010. Umožňuje tooimplement obchodní logiky integrace kompletní identity bez nutnosti toowrite hello zkompilovaný kód.

Nedílnou součást vámi vyžádaných deklarativní zřizování je hello výraz jazyk použitý v toky atributů. Hello použitý jazyk je podmnožinou Microsoft® Visual Basic for Applications (VBA). Tento jazyk slouží v aplikaci Microsoft Office a uživatelé s prostředím jazyka VBScript také rozpozná ho. Hello jazyk výrazů deklarativního zřizování je pouze pomocí funkcí a není strukturovaných jazyk. Neexistují žádné metody nebo příkazy. Místo toho jsou vnořené funkce tooexpress programu toku.

Další podrobnosti najdete v tématu [Vítejte toohello jazyka Visual Basic pro aplikace referenční příručka jazyka pro Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

Hello atributy jsou silného typu. Funkce přijímá pouze atributy hello správného typu. Je také malá a velká písmena. Názvy funkcí a názvy atributů musí mít správná velká a malá písmena nebo dojde k chybě.

## <a name="language-definitions-and-identifiers"></a>Jazyk definic a identifikátory
* Funkce mít název, za nímž následují argumenty v závorce: %{FunctionName/ (argument 1, argument N).
* Atributy jsou určeny hranaté závorky: [attributeName]
* Parametry jsou určeny znaky procenta: % ParameterName %
* Řetězcové konstanty jsou uzavřeny do uvozovek: například "Contoso" (Poznámka: musíte použít rovné uvozovky "" a není inteligentní uvozovky "")
* Číselné hodnoty jsou vyjádřeny bez uvozovek a očekávané toobe decimal. Hexadecimální hodnoty mají předponu & H. Například 98052 & HFF
* Logické hodnoty jsou vyjádřeny pomocí konstant: True, False.
* Předdefinované konstanty a literály jsou vyjádřeny se pouze jejich název: NULL, Line FEED, IgnoreThisFlow

### <a name="functions"></a>Funkce
Deklarativní zřizování používá mnoho funkcí tooenable hello možnost tootransform hodnoty atributu. Tyto funkce mohou být vnořené tak hello výsledek z jednoho funkce je předán v tooanother funkce.

`Function1(Function2(Function3()))`

Hello úplný seznam funkcí najdete v hello [funkce odkaz](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parametry
Parametr je definována pomocí konektoru nebo pomocí správce pomocí prostředí PowerShell. Parametry obvykle obsahují hodnoty, které se liší od toosystem systému, například název hello hello domény hello uživatele nachází v. Tyto parametry můžete použít v toky atributů.

Hello konektor služby Active Directory zadaný hello následující parametry pro příchozí pravidla synchronizace:

| Název parametru | Komentář |
| --- | --- |
| Domain.Netbios |Formát pro rozhraní NetBIOS domény hello aktuálně importována, například FABRIKAMSALES |
| Domain.FQDN |Plně kvalifikovaný název domény formát domény hello aktuálně importována, například sales.fabrikam.com |
| Domain.LDAP |Formát LDAP hello domény aktuálně importována, například řadič domény = prodej, DC = fabrikam, DC = com |
| Forest.Netbios |Formát pro rozhraní NetBIOS hello název doménové struktury aktuálně importována, například FABRIKAMCORP |
| Forest.FQDN |Plně kvalifikovaný název domény formát hello název doménové struktury aktuálně importována, třeba fabrikam.com |
| Forest.LDAP |LDAP formát hello název doménové struktury aktuálně importována, například DC = fabrikam, DC = com |

Hello systém poskytuje hello následující parametr, který použité tooget hello identifikátor hello konektor běží v současné době:  
`Connector.ID`

Tady je příklad, který naplní domény atribut úložiště metaverse hello s názvem netbios hello hello domény, kde se nachází hello uživatele:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operátory
dá se Hello následující operátory:

* **Porovnání**: <, < =, <>, =, >, > =
* **Matematika**: +, -, \*, -
* **Řetězec**: & (zřetězení)
* **Logické**: & & (a), || (nebo)
* **Pořadí vyhodnocení**:)

Operátory jsou vyhodnotí levém tooright a mají hello stejnou prioritou vyhodnocení. To znamená, hello \* (násobitel), nebude hodnocen před - (odčítání). 2\*(5 + 3) není hello stejné jako 2\*5 + 3. Hello hranatých závorek () se používají toochange hello vyhodnocení pořadí při zbývajících pořadí vyhodnocení tooright není vhodné.

## <a name="multi-valued-attributes"></a>Více hodnot atributů
Funkce Hello mohou pracovat s jednou hodnotou i více hodnot atributů. Pro více hodnot atributů hello funkce funguje v každé hodnotě a použije hello stejné funkce tooevery hodnotu.

Například:  
`Trim([proxyAddresses])`Proveďte operace Trim každé hodnoty v atributu proxyAddress hello.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Pro každou hodnotu s @-sign, nahraďte hello domény s @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Vyhledejte hello adresy SIP a odebere ji z hodnot hello.

## <a name="next-steps"></a>Další kroky
* Další informace o hello konfigurační model v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Najdete v tématu Jak deklarativní zřizování je použité out-of-box v [Principy hello výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md).
* V tématu Jak toomake praktická změnit pomocí deklarativní zřizování v [jak toomake toohello změnu výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).

**Témata s přehledem**

* [Synchronizace Azure AD Connect: pochopení a přizpůsobení synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrování místních identit do služby Azure Active Directory](active-directory-aadconnect.md)

**Témata odkazů**

* [Synchronizace Azure AD Connect: odkaz na funkce](active-directory-aadconnectsync-functions-reference.md)

