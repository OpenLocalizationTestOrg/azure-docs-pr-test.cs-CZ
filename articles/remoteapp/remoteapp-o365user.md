---
title: "aaaHow toouse Azure RemoteApp s uživatelských účtů Office 365 | Microsoft Docs"
description: "Zjistěte, jak toouse Azure RemoteApp s uživatelských účtů Office 365"
services: remoteapp
documentationcenter: 
author: piotrci
manager: mbaldwin
ms.assetid: aba70fd3-60b0-4b44-9321-1ab18760ccd5
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d2dbed2a6838adf9bb0f7508eb7dcecb0a74a62f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-remoteapp-with-office-365-user-accounts"></a>Jak toouse Azure RemoteApp s uživatelských účtů Office 365
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Pokud máte předplatné Office 365, máte Azure Active Directory, která ukládá uživatelská jména a hesla použít tooaccess službám Office 365. Například pokud vaši uživatelé aktivují Office 365 ProPlus ověřují vůči toocheck Azure AD pro licence. Většina zákazníků by jako toouse hello stejný adresář s Azure Remoteappem.

Pokud nasazujete Azure RemoteApp používáte s největší pravděpodobností předplatné Azure, který je přidružen jiné službě Azure AD. V pořadí toouse adresáře Office 365, budete potřebovat toomove hello předplatné do adresáře.

Informace o tom, toodeploy Office 365 klientské aplikace, najdete v části [jak toouse předplatné služeb Office 365 s Azure Remoteappem](remoteapp-officesubscription.md).

## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Fáze 1: Registrace vaší bezplatné předplatné Office 365 Azure Active Directory
Pokud používáte hello portál Azure classic, použijte kroky hello v [zaregistrovat bezplatné předplatné služby Azure Active Directory](https://technet.microsoft.com/library/dn832618.aspx) tooget tooyour přístup pro správu Azure AD prostřednictvím hello portálu pro správu Azure. Jako výsledek hello tohoto procesu by měl být schopný toolog do hello portál Azure a najdete v adresáři existuje – v tomto okamžiku neuvidíte mnohem víc vzhledem k tomu, že hello úplné předplatné, které používáte s Azure Remoteappem je v jiném adresáři.

Mějte na paměti, hello jméno a heslo účtu správce hello, kterou jste vytvořili v tomto kroku – bude potřeba v fáze 2.

Pokud používáte hello portálu Azure, podívejte se na [jak tooregister a aktivujte volné Azure Active Directory pomocí portálu Office 365](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-hello-azure-ad-associated-with-your-azure-subscription"></a>Fáze 2: Změna hello Azure AD související s předplatným Azure.
Přidáme toochange vašeho předplatného Azure z jeho aktuálního adresáře do adresáře hello Office 365, které jsme pracovali v fáze 1.

Postupujte podle pokynů hello popsané v [změnu hello klienta Azure Active Directory v Azure Remoteappu](remoteapp-changetenant.md). Věnovat zvláštní pozornost toohello následující kroky:

* Krok #1: Pokud jste nasadili Azure RemoteApp (ARA) v tomto předplatném, ujistěte se, že je všechny uživatelské účty Azure AD ze všech kolekcí ARA nejdříve odebrat, a teprve potom zkusili cokoliv jiného. Alternativně můžete zvážit, odstranění všechny existující kolekce.
* Krok #2: Toto je důležitý krok. Je třeba toouse účet Microsoft (například @outlook.com) jako správce služeb u předplatného hello; důvodem je, že jsme nemůže mít všechny uživatelské účty z hello stávající předplatné Azure AD připojené toohello – v takovém případě jsme nebude možné toomove ho tooa jiné služby Azure AD.
* Krok #4: Při přidávání existující adresář, hello systému se zeptá, toosign se pomocí účtu správce hello tohoto adresáře. Ujistěte se, že účet správce hello toouse z fáze 1.
* Krok #5: Změnit nadřazený adresář hello hello předplatné Office 365 tooyour adresáře. Konečný výsledek Hello by měl být, že v Nastavení -> odběry vaše předplatné uvádí directory hello Office 365. 
  ![Změnit nadřazený adresář hello hello předplatného](./media/remoteapp-o365user/settings.png)

V tomto okamžiku je vaše předplatné Azure Remoteappu související s vaší Office 365, Azure AD; hello existujících uživatelských účtů Office 365 můžete použít s Azure Remoteappem!

