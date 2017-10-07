---
title: "aaaPublish aplikace tooindividual uživatele v kolekci Azure RemoteApp (Preview) | Microsoft Docs"
description: "Zjistěte, jak publikovat aplikace tooindividual uživatele, místo v závislosti na skupin v Azure Remoteappu."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
ms.openlocfilehash: 87b435552ddbfc2c6d03aeb01d95a44985e71f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-tooindividual-users-in-an-azure-remoteapp-collection-preview"></a>Publikování aplikace tooindividual uživatele v kolekci Azure RemoteApp (Preview)
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Tento článek vysvětluje, jak toopublish aplikace tooindividual uživatele v kolekci Azure RemoteApp. Toto je nová funkce v Azure Remoteappu, aktuálně v soukromém náhledu a inovátoři tooselect k dispozici pouze pro účely vyhodnocení.

Původně Azure Remoteappu povolen pouze jeden způsob publikování aplikací: hello správce publikoval aplikace z hello image a nebudou viditelné tooall uživatele v kolekci hello.

Obvyklým scénářem je tooinclude mnoho aplikací do jedné bitové kopie a nasadit jednu kolekci v náklady na správu tooreduce pořadí. Jsou často ne všechny aplikace relevantní tooall uživatelé a Správci by raději toopublish aplikace tooindividual uživatele, aby Ti pak nepotřebných aplikací ve svém kanálu aplikací.

A právě to je nyní v Azure RemoteAppu možné – aktuálně jako omezeně dostupná funkce ve verzi Preview. Zde je uveden stručný přehled hello nové funkce:

1. Kolekce lze nastavit do jednoho ze dvou režimů:
   
   * Hello původní režim kolekce, kde všichni uživatelé v kolekci vidí všechny publikované aplikace. Toto je výchozí režim hello.
   * Hello nový režim aplikací, kde uživatelé vidí pouze aplikace, které byly explicitně přiřazeny toothem
2. Momentálně hello režim aplikací hello lze povolit pouze pomocí rutin prostředí Azure RemoteApp PowerShell.
   
   * Pokud nelze nastavit tooapplication režim, přiřazení uživatelů v kolekci hello spravovat prostřednictvím hello portálu Azure. Přiřazení uživatelů se toobe spravovat pomocí rutin prostředí PowerShell.
3. Uživatelům se zobrazí pouze publikovaná aplikace hello přímo toothem. Ale je stále možné pro uživatele toolaunch hello jiné aplikace, které jsou k dispozici na bitovou kopii hello podle k nim přistupovat přímo v hello operačního systému.
   
   * Tato funkce nenabízí zabezpečené uzamčení aplikací. omezuje pouze jejich viditelnost v kanálu aplikací hello.
   * Pokud potřebujete tooisolate uživatelů z aplikací, musíte pro tento toouse samostatné kolekce.

## <a name="how-tooget-azure-remoteapp-powershell-cmdlets"></a>Jak tooget rutin prostředí Azure RemoteApp PowerShell
tootry hello novou funkci ve verzi preview, budete potřebovat toouse rutin prostředí Azure PowerShell. Aktuálně není možné toouse hello Azure Management portal tooenable hello nový režim publikování aplikací.

Nejprve zkontrolujte, zda máte hello [modul Azure PowerShell](/powershell/azure/overview) nainstalována.

Pak spusťte konzolu prostředí PowerShell hello v režimu správce a spusťte hello následující rutiny:

        Add-AzureAccount

Zobrazí se výzva k zadání vašeho uživatelského jména a hesla pro Azure. Po přihlášení, bude možné toorun rutiny Azure Remoteappu pro svá předplatná Azure.

## <a name="how-toocheck-which-mode-a-collection-is-in"></a>Jak toocheck kterém režimu kolekce je v
Spusťte následující rutinu hello:

        Get-AzureRemoteAppCollection <collectionName>

![Zkontrolujte režim kolekce hello](./media/remoteapp-perapp/araacllelvel.png)

Hello vlastnost AclLevel může mít hello následující hodnoty:

* Kolekce: hello původní režim publikování. Všichni uživatelé vidí všechny publikované aplikace.
* Aplikace: hello nový režim publikování. Uživatelé vidí pouze aplikace hello publikovat přímo toothem.

## <a name="how-tooswitch-tooapplication-publishing-mode"></a>Jak režim publikování tooapplication tooswitch
Spusťte následující rutinu hello:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Stav publikování jednotlivých aplikací bude zachován: původně všichni uživatelé uvidí všechny původně publikované aplikace hello.

## <a name="how-toolist-users-who-can-see-a-specific-application"></a>Jak toolist uživatelé, kteří mohou vidět konkrétní aplikaci
Spusťte následující rutinu hello:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Rutina Vypíše seznam všech uživatelů, kteří mohou vidět hello aplikaci.

Poznámka: Zobrazí se hello aliasy aplikací (nazývané v hello syntaxi výše "app alias") spuštěním příkazu Get-AzureRemoteAppProgram - Názevkolekce <collectionName>.

## <a name="how-tooassign-an-application-tooa-user"></a>Jak tooassign uživatel tooa aplikace
Spusťte následující rutinu hello:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Hello uživatel nyní uvidí aplikaci hello v klientovi Azure Remoteappu hello a bude moct tooconnect tooit.

## <a name="how-tooremove-an-application-from-a-user"></a>Jak tooremove aplikace od uživatele.
Spusťte následující rutinu hello:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Poskytnutí zpětné vazby
Oceníme vaše názory a návrhy týkající se této funkce ve verzi Preview. Vyplňte prosím hello [průzkum](http://www.instant.ly/s/FDdrb) toolet nám vědět, co si myslíte.

## <a name="havent-had-a-chance-tootry-hello-preview-feature"></a>Neměli jste možnost tootry hello Ukázková funkce?
Pokud jste se dosud neúčastnili hello preview ještě, použijte prosím tento [průzkum](http://www.instant.ly/s/AY83p) toorequest přístup.

