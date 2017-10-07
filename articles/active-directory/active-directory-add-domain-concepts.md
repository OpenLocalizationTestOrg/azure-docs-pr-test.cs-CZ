---
title: "aaaConceptual Přehled vlastních názvů domén v Azure Active Directory | Microsoft Docs"
description: "Vysvětluje hello konceptuální architektura pro používání vlastních názvů domén v Azure Active directory, včetně federace pro jednotné přihlašování"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: fd0c5def-0da2-43af-81bc-76f4cfe86afd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 0a3454ae6b733a8a13a71925df3cc664063f388e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="conceptual-overview-of-custom-domain-names-in-azure-active-directory"></a>Koncepční přehled vlastních názvů domén v Azure Active Directory
Název domény může být důležité identifikátor pro mnoho directory prostředky, jako součást:

* Pro uživatele uživatelského jména nebo e-mailové adresy
* Hello adresy pro skupinu
* identifikátor ID URI aplikace Hello pro aplikaci

Název domény, který je již ověřit, že toobe vlastníkem hello adresáře, která obsahuje hello prostředků může obsahovat prostředků v Azure Active Directory (Azure AD). Globální správce můžete provádět úlohy správy domény ve službě Azure AD.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Jak toomanage vaše názvů domén v Centru pro správu hello Azure AD, najdete v části [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).

Názvy domén ve službě Azure AD jsou globálně jedinečný. Vlastní název domény lze pouze jeden klient Azure AD v čase. Pokud jeden adresář Azure AD ověří název domény, můžete žádné další adresář Azure AD ověřit nebo použít stejný název domény.

## <a name="initial-and-custom-domain-names"></a>Názvy počáteční a vlastní domény
Každý název domény ve službě Azure AD je buď počáteční název domény, nebo vlastní název domény.

Každý Azure AD se dodává s počáteční název domény v hello formuláře contoso.onmicrosoft.com. Tento název třetí úrovně domény, v tomto příkladu "contoso.onmicrosoft.com," byl vytvořen, když hello adresář byl vytvořen, obvykle hello správce, který vytvořil adresář hello. Hello počáteční název domény pro adresář nelze změnit ani odstranit. Hello počáteční název domény, zatímco plně funkční, je určena, že je ověřený především toobe používat jako mechanismus samozaváděcí dokud vlastního názvu domény.

Ve většině produkční prostředí adresář má alespoň jeden ověřené vlastní domény, například "contoso.com", a je tento vlastní doménu, která je viditelná tooend uživatele. Vlastní název domény je název domény, který je ve vlastnictví a použít dané organizace, jako je například "contoso.com", pro účely, například hostování svého webu. Tento název domény je známé tooemployees, protože je součástí hello uživatelské jméno, že použijete toosign v podnikové síti toohello nebo toosend a načtení e-mailů.

Před použitím Azure AD, hello vlastní název domény musí být přidané tooyour adresář a ověřit.

## <a name="verified-and-unverified-domain-names"></a>Názvy ověřeny a neověřených domén
Hello počáteční název domény pro adresář je implicitně vyhodnotit, jak ověřit pomocí služby Azure AD. Když správce přidá tooan název vlastní doménu služby Azure AD, je nejprve v neověřené stavu. Azure AD neumožní jakékoli toouse directory prostředky název neověřené domény. To zajistí, že pouze jeden adresář můžete použít název konkrétní domény a že hello organizace používá název domény hello ve skutečnosti vlastní název domény.

Azure AD ověřuje vlastnictví názvu domény tak, že vyhledá konkrétní položky v souboru zóny hello domain name service (DNS) pro název domény hello. tooverify vlastnictví názvu domény, správce získá položky DNS hello z Azure AD, Azure AD bude hledat a přidá tento záznam toohello souboru zóny DNS pro název domény hello. soubor zóny DNS Hello zachovaný hello registrátorem názvu domény pro tuto doménu. Hello kroky tooverify domény jsou uvedené v článku hello [přidání adresář Azure AD tooyour vlastní domény](active-directory-add-domain.md).

Přidání souboru zóny toohello záznam DNS pro název domény hello nemá vliv na jiné domény služby, například e-mailu nebo publikování na webu.

## <a name="federated-and-managed-domain-names"></a>Názvy domén federované a spravované
Vlastní název domény ve službě Azure AD může být uživatelů nakonfigurovanou toogive federované přihlašování prostředí mezi vaší místní služby Active Directory a Azure AD. Konfigurace domény pro federace vyžaduje aktualizaci tooprivileged prostředky v Azure AD a také tooyour Windows Server Active Directory. Konfiguruje federované domény musí být dokončena z Azure AD Connect nebo pomocí prostředí PowerShell. Federaci vlastní domény nejde inicializovat z hello portál Azure classic. [Podívejte se na toto video toolearn o konfiguraci služby AD FS pro uživatele přihlásit se pomocí Azure AD Connect](http://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect).

Domény, které nejsou federované se někdy označuje jako spravované domény. Hello počáteční domény pro adresář služby Azure AD je implicitně vyhodnocena jako spravované domény.

## <a name="primary-domain-names"></a>Názvy primární doménu
Hello primární název domény pro adresář je hello název domény, který je předem vybraný jako výchozí hodnota hello hello domény část hello uživatelské jméno, když správce vytvoří nového uživatele v hello [portál Azure](https://portal.azure.com/), nebo jiném portálu Například portál pro správu hello Office 365 nebo portálu Microsoft Intune hello. Adresář může mít pouze jeden název domény. Správce může změnit hello primární doménu název toobe všechny ověřené vlastní doménu, která není federovaný nebo toohello počáteční domény.

## <a name="domain-names-in-azure-ad-and-other-microsoft-online-services"></a>Názvy domén ve službě Azure AD a dalších služeb Microsoft Online Services
Název domény musí ověřit ve službě Azure AD, než mohou být využívána jiné Online službu Microsoftu, jako je Exchange Online, SharePoint Online a Intune. Tyto další služby obvykle vyžadují správce tooadd jeden nebo více záznamy DNS, které jsou konkrétní toohello služby.

Webové aplikace Azure používá vlastní mechanismus tooverify vlastnictví domény. Domény musí ověřit pro použití s Azure AD i v případě, že dříve ověření pro použití ve webové aplikace Azure v rámci předplatného, které jsou závislé na této službě Azure AD. Webové aplikace Azure můžete použít název domény, který byl ověřen v jiném adresáři z hello adresář, který zabezpečuje hello webové aplikace.

## <a name="managing-domain-names"></a>Správa názvů domén
Úlohy správy domény je možné dokončit, z hello portál Azure classic a z prostředí PowerShell. Mnoho úloh je možné provést pomocí hello Azure AD Graph API.

* [Přidání a ověření vlastního názvu domény](active-directory-add-domain.md)
* [Správa domén v hello portál Azure classic](active-directory-add-manage-domain-names.md)
* [Pomocí názvů domén toomanage prostředí PowerShell ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Pomocí názvů domén toomanage hello Azure AD Graph API v Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

