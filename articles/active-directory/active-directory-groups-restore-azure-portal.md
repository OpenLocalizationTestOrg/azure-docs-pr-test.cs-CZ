---
title: "skupiny aaaRestore odstraněné Office 365 ve službě Azure Active Directory | Microsoft Docs"
description: "Jak toorestore odstraněné skupině, zobrazit obnovitelné skupiny a permamnently odstranit skupinu v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: curtand
ms.openlocfilehash: 4e6650d64aa19f0c909f1c511f48681de8032f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a>Obnovit odstraněné skupiny Office 365 ve službě Azure Active Directory

Při odstranění skupiny služby Office 365 v hello Azure Active Directory (Azure AD), hello odstranit skupina je zachované, ale nejsou viditelné po dobu 30 dnů od data odstranění hello. Toto je tak, aby hello skupiny a její obsah se dají obnovovat v případě potřeby. Tato funkce je omezen výhradně tooOffice 365 skupiny ve službě Azure AD. Není k dispozici pro skupiny zabezpečení a distribučních skupin.

> [!NOTE] 
> Nepoužívejte `Remove-MsolGroup` protože skupiny hello trvale odstraní. Vždy používat `Remove-AzureADMSGroup` skupiny toodelete O365. 

Hello oprávnění požadované toorestore skupiny může být libovolná z hello následující:

Role  | Oprávnění 
--------- | ---------
Správce společnosti, partnera Tier2 podpory a správci služby InTune | Můžete obnovit všechny odstraněné skupiny Office 365 
Podpora uživatelského účtu správce a Tier1 partnera | Můžete obnovit všechny odstraněné skupiny Office 365 s výjimkou těchto toohello přiřazenou roli správce společnosti 
Uživatel | Můžete obnovit všechny odstraněné skupiny Office 365, který se ve vlastnictví 


## <a name="view-hello-deleted-office-365-groups-that-are-available-toorestore"></a>Zobrazení hello odstranit skupiny Office 365, které jsou k dispozici toorestore
Hello následující rutiny lze použít tooview hello odstranit skupiny tooverify, který hello jednu nebo ty, které, které vás zajímají nebyly dosud vyprázdnila trvale. Tyto rutiny jsou součástí hello [modulu Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureAD/). Další informace o tomto modulu najdete v hello [Azure Active Directory PowerShell verze 2](/powershell/azure/install-adv2?view=azureadps-2.0) článku.

1.  Spusťte následující rutinu toodisplay odstranit skupiny Office 365 ve vašem klientovi, které jsou stále k dispozici toorestore hello.
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  Případně pokud víte hello objectID určité skupiny (a můžete ho získat prostřednictvím hello rutiny v kroku 1), spusťte hello následující rutiny tooverify, který hello konkrétní odstraněné skupiny nebyl ještě vyprázdnila trvale.
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-toorestore-your-deleted-office-365-group"></a>Jak toorestore vaše odstraněné skupiny Office 365
Jakmile si ověříte, že dané skupiny hello je stále k dispozici toorestore, obnovení hello odstranit skupiny s jedním z následujících kroků hello. Pokud skupina hello obsahuje dokumenty, SP lokalit nebo jiné trvalé objekty, může trvat až too24 hodin toofully obnovení skupiny a její obsah.

1.  Spuštění hello následující rutiny toorestore hello skupiny a její obsah.
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

Alternativně hello následující rutiny můžete spustit toopermanently odstranit hello odstranit skupinu.
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a>Jak to šlo znáte?
tooverify jste úspěšně obnovil skupiny služby Office 365, spusťte hello `Get-AzureADGroup –ObjectId <objectId>` rutiny toodisplay informace o skupině hello. Po obnovení hello dokončení žádosti:
- Skupina Hello se zobrazí v levém navigačního panelu hello na Exchange
- Zobrazí se v Planner Hello plán pro skupinu hello
- Všechny weby služby Sharepoint a veškerý jeho obsah bude k dispozici
- skupiny Hello je přístupná ze všech koncových bodů hello Exchange a další úlohy Office 365, které podporují skupiny Office 365


## <a name="next-steps"></a>Další kroky
Tyto články poskytují další informace o skupinách Azure Active Directory.

* [Zobrazení existujících skupin](active-directory-groups-view-azure-portal.md)
* [Spravovat nastavení skupiny](active-directory-groups-settings-azure-portal.md)
* [Správa členů skupiny](active-directory-groups-members-azure-portal.md)
* [Správa členství ve skupině](active-directory-groups-membership-azure-portal.md)
* [Dynamická pravidla pro uživatele ve skupině pro správu](active-directory-groups-dynamic-membership-azure-portal.md)
