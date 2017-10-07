---
title: "aaaHow Azure předplatné propojeno se službou Azure Active Directory | Microsoft Docs"
description: "Přihlášení tooMicrosoft Azure a související problémy, jako je například hello vztah mezi předplatným Azure a Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: bc4773c2-bc4a-4d21-9264-2267065f0aea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/24/2017
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 4f831cfb972efec57083fcaa63adb43fde7b2faf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-azure-subscriptions-are-associated-with-azure-active-directory"></a>Jak je předplatné Azure propojeno se službou Azure Active Directory
Tento článek obsahuje informace o hello vztah mezi předplatným Azure a Azure Active Directory (Azure AD) a jak tooadd existující adresář Azure AD tooyour předplatné.

## <a name="your-azure-subscriptions-relationship-tooazure-ad"></a>Vztah tooAzure předplatné Azure AD
Vaše předplatné Azure má vztah důvěryhodnosti s Azure AD, která znamená, že hello directory tooauthenticate uživatelů, služeb a zařízení. Několik předplatných může důvěřovat hello stejný adresář, ale každé předplatné důvěřuje pouze jednomu adresáři. 

Hello vztah důvěryhodnosti, který má předplatné s adresářem je na rozdíl od hello vztah, který má s jiným prostředkům v Azure (weby, databáze atd.). Pokud platnost předplatného vyprší, přístup k toohello další prostředky přidružené k předplatnému hello také zastaví. Ale ve službě Azure zůstane adresář služby Azure AD, a můžete přidružit jiné předplatné adresáře a spravovat adresář hello pomocí hello nové předplatné.

![Diagram přidružení předplatných](./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png)

Všichni uživatelé mají jeden domovský adresář, který je ověřuje, ale mohou být také hosty v dalších adresářích. Ve službě Azure AD najdete v hello adresáře, ve kterých je váš uživatelský účet hosta nebo člena.

## <a name="azure-ad-and-cloud-service-subscriptions"></a>Předplatná cloudových služeb a Azure AD
Azure AD poskytuje hello základní adresáře a identity, možnosti správy většinu cloudových služeb společnosti Microsoft, včetně:

* Azure
* Microsoft Office 365
* Microsoft Dynamics CRM Online
* Microsoft Intune

Hello bezplatné služby Azure AD získáte při registraci pro některý z těchto cloudových služeb Microsoftu. Pokud chcete tooadd adresář Azure AD tooan další předplatné Azure, musí být podepsané pomocí účtu Microsoft. Pokud jste přihlášení tooAzure s pracovní nebo školní účet, nelze přidat existující adresář tooan předplatné Azure, protože váš pracovní nebo školní účet nelze ověřit přímo v Azure. 

## <a name="tooadd-an-existing-subscription-tooyour-azure-ad-directory"></a>tooadd existující adresář Azure AD tooyour předplatného
Přihlaste se pod účtem, který existuje v obou hello aktuálního adresáře, ke které hello odběr přidružen a v adresáři hello chcete tooadd jeho. 

1. Přihlaste se toohello [centra účtů Azure](https://account.windowsazure.com/Home/Index) pomocí účtu, který je hello správce účtu předplatného hello jejichž vlastnictví chcete tootransfer.
2. Ujistěte se, že hello uživatele, který budete chtít vlastník předplatného toobe hello je v adresáři hello cílem.
3. Klikněte na **Převod vlastnictví předplatného**.
4. Zadejte příjemce hello. příjemce Hello automaticky získá e-mail s odkazem přijetí.
5. příjemce Hello klikne na odkaz hello a postupuje hello pokynů, včetně zadáním jejich platebních údajů. Pokud je příjemce hello úspěšná, se přenáší hello předplatného. 
6. Výchozí adresář Hello hello předplatného se změní adresář toohello tento uživatel hello je v.


## <a name="suggestions-toomanage-both-a-subscription-and-a-directory"></a>Návrhy toomanage předplatné a adresářů
Hello správní role pro předplatné Azure spravovat prostředky svázané toohello předplatného Azure. Tato část vysvětluje hello rozdíly mezi správci předplatného služby Azure a správci adresáře Azure AD. Role pro správu a další návrhy k jejich použití toomanage předplatného jsou popsané v kapitole [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles.md).

Ve výchozím nastavení jsou přiřazené role Správce služeb hello při registraci. Pokud ostatní musíte toosign v a získat přístup ke službám pomocí hello stejného předplatného, můžete je přidat jako spolusprávce. 

Azure AD má jinou sadu directory hello toomanage správní role a funkce související s identity. Hello globální správce adresáře může například přidat uživatele a skupiny toohello adresáře nebo vyžadovat vícefaktorové ověřování pro uživatele. Uživatel, který vytvoří adresář je přiřazena role globálního správce toohello a jejich můžete přiřadit role správců tooother uživatele. Správní role služby Azure AD jsou také používány dalšími službami, například Office 365 nebo Microsoft Intune. 

Správci předplatného Azure a správci adresáře Azure AD představují dva samostatné koncepty. 
* Správci předplatného služby Azure můžou spravovat prostředky v Azure a pomocí služby Azure AD v hello portálu Azure (protože portál Azure, samotné hello je prostředek služby Azure). 
* Správci adresáře mohou spravovat vlastnosti jenom v hello adresář Azure AD.

Jedna osoba může mít obě role, ale není to nutné. Globální správce adresáře nemůže být přiřazen jako správce nebo spolusprávce služby v předplatném Azure ani naopak. Aniž by musel být správcem předplatného hello, hello uživatel může přihlásit toohello portálu Azure, ale nemůže spravovat hello adresáře pro toto předplatné hello portálu. Tento uživatel však můžete spravovat adresářů pomocí jiných nástrojů, například Azure AD PowerShell nebo hello Centrum pro správu Office 365.

## <a name="next-steps"></a>Další kroky
* Další informace o toolearn jak správci toochange pro předplatné Azure, najdete v části [přenos vlastnictví tooanother účtu předplatného Azure](../billing/billing-subscription-transfer.md)
* toolearn Další informace o tom, jak je přístup k prostředkům řídí ve službě Microsoft Azure, najdete v části [Principy přístupu k prostředkům v Azure](active-directory-understanding-resource-access.md)
* Další informace o tom, tooassign role ve službě Azure AD, najdete v části [přiřazení rolí správce v Azure Active Directory](active-directory-assign-admin-roles-azure-portal.md)

<!--Image references-->
[1]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_PassThruAuth.png
[2]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_OrgAccountSubscription.png
[3]: ./media/active-directory-how-subscriptions-associated-directory/WAAD_SignInDisambiguation.PNG
