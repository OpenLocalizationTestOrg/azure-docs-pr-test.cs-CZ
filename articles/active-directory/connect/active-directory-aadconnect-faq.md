---
title: "Azure Active Directory Connect: Časté otázky – | Microsoft Docs"
description: "Tato stránka obsahuje časté otázky k Azure AD Connect."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 39c0b127d3dcf6f45607ad8b4647a9ad79dfc732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Nejčastější dotazy pro Azure Active Directory Connect

## <a name="general-installation"></a>Obecné instalace
**Otázka: bude instalace fungovat, pokud hello Azure AD globálního správce má 2FA povolené?**  
S hello sestavení z února 2016 to je podporováno.

**Otázka: je způsob, jak tooinstall bezobslužné Azure AD Connect?**  
Je pouze podporované tooinstall Azure AD Connect pomocí Průvodce instalací hello. Bezobslužné a bezobslužné instalace není podporována.

**Otázka: je nutné do doménové struktury, kde nelze kontaktovat jednu doménu. Instalace Azure AD Connect**  
S hello sestavení z února 2016 to je podporováno.

**Otázka: hello pracovní agenta stavu služby AD DS na jádro serveru?**  
Ano. Po instalaci agenta hello, může dokončení procesu registrace hello pomocí hello následující rutiny prostředí PowerShell: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Síť
**Otázka: je nutné bránu firewall, síťové zařízení, nebo něco jiného, který omezuje hello maximální dobu, po připojení můžete zůstat otevřené v mé síti. Jak dlouho mají Moje prahová hodnota časového limitu straně klienta se při použití Azure AD Connect?**  
Všechny síťové softwaru, fyzických zařízení nebo cokoliv jiného, co omezení, které hello maximální dobu, které připojení zůstat otevřené měli používat prahovou hodnotu alespoň 5 minut (300 sekund) pro připojení mezi hello serveru, kde hello Azure AD Connect je nainstalován klient a Azure Active Directory. To platí také nástrojů pro synchronizaci tooall dřív vydané Microsoft Identity.

**Otázka: jsou podporovány domény SLD (jedné domény štítek)?**  
Ne, Azure AD Connect místními doménovými strukturami nebo doménami pomocí domény SLD nepodporuje.

**Otázka: je "s tečkami" s názvem pro rozhraní NetBios podporovány?**  
Ne, Azure AD Connect nepodporuje místními doménovými strukturami nebo doménami, kde název pro rozhraní NetBios hello obsahuje tečku "." v názvu hello.

## <a name="federation"></a>Federace
**Otázka: Co mám dělat, když se zobrazí e-mailu, výzva toorenew Moje certifikát Office 365**  
Použít hello pokyny, které je uvedené v hello [obnovení certifikátů](active-directory-aadconnect-o365-certs.md) téma na tom, jak toorenew hello certifikátu.

**Otázka: je nutné "Automaticky aktualizovat předávající strany" sadu O365 předávající strany. Je nutné provést tootake žádnou akci při navyšování Moje automaticky podpisový certifikát tokenu?**  
Použít hello pokyny, které je popsané v článku hello [obnovení certifikátů](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Prostředí
**Otázka: je it podporované toorename hello server po instalaci Azure AD Connect?**  
Ne. Změna názvu serveru hello bude příčina hello synchronizační modul toonot možné tooconnect toohello SQL database a hello služba nebude možné toostart.

## <a name="identity-data"></a>Data identit
**Otázka: hello atribut UPN (userPrincipalName) ve službě Azure AD neodpovídá hello místní UPN - proč?**  
Najdete v těchto článcích:

* [Uživatelská jména v Office 365, Azure nebo Intune se neshodují hello místní UPN nebo alternativním přihlašovacím ID](https://support.microsoft.com/en-us/kb/2523192)
* [Změny nejsou synchronizovány pomocí nástroje Azure Active Directory Sync hello po změně hello UPN uživatel účet toouse jiné federované domény](https://support.microsoft.com/en-us/kb/2669550)

Můžete také konfigurovat Azure AD tooallow hello synchronizační modul tooupdate hello userPrincipalName, jak je popsáno v [funkce služby Azure AD Connect sync](active-directory-aadconnectsyncservice-features.md).

**Otázka: je it podporované toosoft shodu místní objekty AD skupiny nebo kontaktu s existující objekty Azure AD skupiny nebo kontaktu?**  
Ne, to není aktuálně podporováno.

**Otázka: je it podporované toomanually nastavte atribut ImmutableId na existující Azure AD skupiny nebo kontaktu objekty toohard shodovat se tooon místní AD skupiny nebo kontaktu objekty?**  
Ne, to není aktuálně podporováno.



## <a name="custom-configuration"></a>Vlastní konfigurace
**Otázka: kde jsou hello rutiny prostředí PowerShell pro Azure AD Connect zdokumentovaný?**  
S výjimkou hello hello rutiny popsané v této lokalitě nejsou podporované jinými rutinami prostředí PowerShell nalezen ve službě Azure AD Connect pro použití zákazníka.

**Otázka: je možné použít "Serveru serveru pro export/import" nalezena v *Synchronization Service Manager* konfigurace toomove mezi servery?**  
Ne. Tato možnost nebude načíst všechna nastavení konfigurace a by se neměla používat. Místo toho by měla použijte hello Průvodce toocreate hello základní konfigurace na druhý server hello a pomocí hello synchronizační pravidlo editor toogenerate prostředí PowerShell skripty toomove žádné vlastní pravidlo mezi servery. V tématu [kyvu migrace](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**Otázka: je možné hesla do mezipaměti pro stránku hello Azure přihlášení a může to možné, protože obsahuje element input pro heslo pomocí funkce Automatické dokončování hello = "false" atribut?**</br>
Aktuálně nepodporujeme změny atributy HTML hello hello vstupní pole pro heslo, včetně hello automatického dokončování značky. Právě pracujeme na funkce, která vám umožní pro vlastní Javascript, která vám umožní tooadd každé pole atributu toohello heslo. To by měl být k dispozici novější součástí 2017.

**Otázka: na hello Azure přihlašovací stránce se zobrazí uživatelských jmen pro uživatele, kteří mají dříve úspěšně přihlášení.  Lze toto chování vypnout?**</br>
Aktuálně nepodporujeme změny atributů HTML hello hello přihlašovací stránky. Právě pracujeme na funkce, která vám umožní pro vlastní Javascript, která vám umožní tooadd každé pole atributu toohello heslo. To by měl být k dispozici novější součástí 2017.

**Otázka: je způsob, jak tooprevent souběžných relací?**</br>
Ne.



## <a name="troubleshooting"></a>Řešení potíží
**Otázka: jak můžete získat pomoc s Azure AD Connect?**

[Hledání hello znalostní báze Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Hledání hello znalostní báze Microsoft Knowledge Base (KB) technické řešení toocommon opravu problémů o podpoře pro Azure AD Connect.

[Fóra Microsoft Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Můžete vyhledat a procházet technical otázky a odpovědi od komunity hello nebo položte svou vlastní otázku kliknutím [zde](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD Connect zákaznickou podporu](https://manage.windowsazure.com/?getsupport=true)

* Použijte tento odkaz tooget podporu prostřednictvím hello portálu Azure.

