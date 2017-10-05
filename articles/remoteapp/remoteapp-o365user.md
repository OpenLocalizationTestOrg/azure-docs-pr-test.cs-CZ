---
title: "Jak používat Azure RemoteApp s uživatelských účtů Office 365 | Microsoft Docs"
description: "Naučte se používat Azure RemoteApp s uživatelských účtů Office 365"
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
ms.openlocfilehash: 1bc8949c236afd03415f961cf7a657d4d3926b07
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-remoteapp-with-office-365-user-accounts"></a>Jak používat Azure RemoteApp s uživatelských účtů Office 365
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Podrobnosti najdete v tomto [oznámení](https://go.microsoft.com/fwlink/?linkid=821148).
> 
> 

Pokud máte předplatné služeb Office 365, máte Azure Active Directory, která ukládá uživatelská jména a hesla, používá pro přístup ke službám Office 365. Například když vaši uživatelé aktivují Office 365 ProPlus ověření služby Azure AD zkontrolujte licence. Většina zákazníků chcete použít stejný adresář s Azure Remoteappem.

Pokud nasazujete Azure RemoteApp používáte s největší pravděpodobností předplatné Azure, který je přidružen jiné službě Azure AD. Chcete-li použít adresáře Office 365, musíte přesunout předplatné Azure, které do této složky.

Informace o tom, jak nasadit aplikace klienta Office 365 najdete v tématu [jak používat předplatné služeb Office 365 s Azure Remoteappem](remoteapp-officesubscription.md).

## <a name="phase-1-register-your-free-office-365-azure-active-directory-subscription"></a>Fáze 1: Registrace vaší bezplatné předplatné Office 365 Azure Active Directory
Pokud používáte portál Azure classic, použijte kroky v [zaregistrovat bezplatné předplatné služby Azure Active Directory](https://technet.microsoft.com/library/dn832618.aspx) získat přístup pro správu do služby Azure AD pomocí portálu pro správu Azure. V důsledku tohoto procesu bude schopen Přihlaste se k portálu Azure a najdete v adresáři existuje – v tomto okamžiku neuvidíte mnohem víc vzhledem k tomu, že úplné předplatné, které používáte s Azure Remoteappem je v jiném adresáři.

Mějte na paměti, jméno a heslo účtu správce, kterou jste vytvořili v tomto kroku – bude potřeba v fáze 2.

Pokud používáte portál Azure, podívejte se na [k registraci a aktivaci volné Azure Active Directory pomocí portálu Office 365](http://azureblogger.com/2016/01/how-to-register-and-activate-a-free-azure-active-directory-using-office-365-portal/).

## <a name="phase-2-change-the-azure-ad-associated-with-your-azure-subscription"></a>Fáze 2: Změňte Azure AD spojené s předplatným Azure.
Přidáme změnit vašeho předplatného Azure z jeho aktuálního adresáře do adresáře Office 365, které jsme pracovali v fáze 1.

Postupujte podle pokynů v tématu [změna klienta Azure Active Directory v Azure Remoteappu](remoteapp-changetenant.md). Věnujte zvláštní pozornost následující kroky:

* Krok #1: Pokud jste nasadili Azure RemoteApp (ARA) v tomto předplatném, ujistěte se, že je všechny uživatelské účty Azure AD ze všech kolekcí ARA nejdříve odebrat, a teprve potom zkusili cokoliv jiného. Alternativně můžete zvážit, odstranění všechny existující kolekce.
* Krok #2: Toto je důležitý krok. Je nutné použít účet Microsoft (například @outlook.com) jako správce služeb v předplatném; důvodem je, že jsme nemůže mít všechny uživatelské účty z existující Azure AD, které jsou připojené k předplatnému – v takovém případě jsme nebudete moci přesunout do jiné služby Azure AD .
* Krok #4: Při přidávání existující adresář, systém se zeptá se přihlásit pomocí účtu správce pro tento adresář. Ujistěte se, že pomocí účtu správce z fáze 1.
* Krok #5: Změňte nadřazený adresář předplatného do vašeho adresáře Office 365. Konečný výsledek by měl být, že v Nastavení -> odběry vaše předplatné uvádí adresáři Office 365. 
  ![Změnit nadřazený adresář předplatného](./media/remoteapp-o365user/settings.png)

V tomto okamžiku je vaše předplatné Azure Remoteappu související s vaší Office 365, Azure AD; můžete použít existující uživatelských účtů Office 365 s Azure Remoteappem!

