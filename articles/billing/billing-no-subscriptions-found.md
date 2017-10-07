---
title: "odběry aaaNo zjištěna chyba při akci toosign v tooAzure portálu nebo centra účtů Azure | Microsoft Docs"
description: "Poskytuje hello řešení problému, ve kterém žádné předplatné Nenalezeno, dojde k chybě při přihlášení tooAzure portálu nebo centra účtů Azure."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: genli
ms.openlocfilehash: def4d4a1f883dd948fe8132f2d85abc4c23ae624
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="no-subscriptions-found-error-in-azure-portal-or-azure-account-center"></a>Žádná předplatná nalezena chyba na portálu Azure nebo centra účtů Azure
Může zobrazit chybová zpráva "Nebyla nalezena žádná předplatná", když zkusíte toosign v toohello [portál Azure](https://portal.azure.com/) nebo hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions). Tento článek poskytuje řešení tohoto problému.

## <a name="symptom"></a>Příznaky

Když se pokusíte toosign v toohello [portál Azure](https://portal.azure.com/) nebo hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions), obdržíte hello následující chybová zpráva: "Nebyla nalezena žádná předplatná".

## <a name="cause"></a>Příčina

K tomuto problému dochází, pokud váš účet nemá dostatečná oprávnění. 

## <a name="solution"></a>Řešení

Ujistěte se, přihlásit se jako správce správné hello. Účet správce můžete přistupovat pouze centra účtů hello. Správci služby (SA) a Spolusprávci (CA) mají přístup oprávnění pouze toohello portál Azure nebo hello portál Azure classic.

### <a name="scenario-1-error-message-is-received-in-hello-azure-portalhttpsportalazurecom"></a>Scénář 1: Chybová zpráva se získaly v hello [portálu Azure](https://portal.azure.com)

toofix tohoto problému:

* Ujistěte se, že hello správný, že adresáře Azure je vybrán kliknutím svůj účet na vpravo nahoře hello.

  ![Vyberte hello adresář v hello top napravo od hello portálu Azure](./media/billing-no-subscriptions-found/directory-switch.png)

* Pokud hello právo Azure directory je vybrána, ale stále hello chybová zpráva, [byl váš účet přidán jako vlastníka](billing-add-change-azure-subscription-administrator.md).

### <a name="scenario-2-error-message-is-received-in-hello-azure-account-centerhttpsaccountwindowsazurecomsubscriptions"></a>Scénář 2: Chybová zpráva se získaly v hello [centra účtů Azure](https://account.windowsazure.com/Subscriptions)

Zkontrolujte, zda text hello účet, který jste použili je hello správce účtu. je tooverify, kdo hello účet správce, postupujte takto:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com).
2. V nabídce centra hello vyberte **předplatné**.
3. Vyberte předplatné hello má toocheck a pak vyberte **nastavení**.
4. Vyberte **vlastnosti**. Správce účtu Hello hello předplatného se zobrazí v hello **správce účtu** pole.

## <a name="need-help-contact-support"></a>Potřebujete pomoct? Obraťte se na podporu.
Pokud stále potřebujete pomoc, [obraťte se na podporu](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409) tooget rychle vyřešit problém. 
