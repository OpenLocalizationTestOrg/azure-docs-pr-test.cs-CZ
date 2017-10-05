---
title: "Správa vlastních názvů domén v Azure Active Directory | Microsoft Docs"
description: "Koncepty správy a postupy pro správu vlastní doménu v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 5ae19bb370064de96cf466ca09b13d02563d65a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Správa vlastních názvů domén v Azure Active Directory
Název domény může být důležité identifikátor pro mnoho directory prostředky, jako součást:

* Pro uživatele uživatelského jména nebo e-mailové adresy
* Adresa pro skupinu
* Identifikátor ID URI aplikace pro aplikaci

Prostředek v Azure Active Directory (Azure AD) může zahrnovat název domény, který je již ověřit vlastnit adresář, který obsahuje prostředek. Globální správce můžete provádět úlohy správy domény ve službě Azure AD.

> [!IMPORTANT]
> Společnost Microsoft doporučuje při správě služby Azure AD používat [centrum pro správu Azure AD](https://aad.portal.azure.com) na webu Azure Portal namísto používání portálu Azure Classic, na který odkazuje tento článek. Jak spravovat názvy domén v Centru správy služby Azure AD, najdete v článku [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Nastavte název primární domény pro váš adresář Azure AD
Při vytváření adresáře, název domény, jako je například 'contoso.onmicrosoft.com', je také název primární domény pro váš adresář. Primární doména je výchozí název domény pro nového uživatele, když vytvoříte nový uživatel v [portál Azure classic](https://manage.windowsazure.com/), nebo dalších portálech, jako je například portál pro správu Office 365. To zjednodušuje proces pro správce k vytváření nových uživatelů na portálu.

Chcete-li změnit název primární domény pro váš adresář:

1. Přihlaste se k [portálu Azure Classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.
2. Vyberte **služby Active Directory** na levém navigačním panelu.
3. Otevřete svůj adresář.
4. Vyberte **domén** kartě.
5. Vyberte **změnu primární** tlačítka na panelu příkazů.
6. Vyberte doménu, která mají být nové primární domény pro váš adresář.

Můžete změnit název primární domény pro váš adresář na všechny ověřené vlastní doménu, která není federovaný. Změna primární domény pro váš adresář nedojde ke změně uživatelská jména pro všechny stávající uživatele.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Přidat vlastní názvy domén do služby Azure AD
Pro každý adresář Azure AD můžete přidat až 900 názvy vlastních domén. Proces [, přidejte další vlastní domény název](active-directory-add-domain.md) je stejný pro první vlastní název domény.

## <a name="add-subdomains-of-a-custom-domain"></a>Přidat subdomény vlastní domény
Pokud chcete přidat název domény třetí úrovně například 'europe.contoso.com' do vašeho adresáře, měli byste nejprve přidat a ověřit domény druhé úrovně, například contoso.com. Subdoméně bude automaticky ověřit pomocí služby Azure AD. Pokud chcete zjistit, zda byla ověřena subdomény, kterou jste právě přidali, aktualizujte stránku v prohlížeči, který obsahuje seznam domén ve vašem adresáři.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Co dělat, když změníte registrátora DNS pro vlastní název domény
Pokud změníte registrátora DNS pro vlastní název domény, můžete nadále používat vlastní název domény se službou Azure AD samotné bez přerušení a bez další konfigurace úlohy. Pokud používáte vlastní název domény pro Office 365, Intune nebo jiné služby, které jsou závislé na vlastních názvů domén ve službě Azure AD, naleznete v dokumentaci pro tyto služby.

## <a name="delete-a-custom-domain-name"></a>Odstranit vlastní název domény
Vlastní název domény můžete odstranit ze služby Azure AD, pokud vaše organizace už používá název domény, nebo pokud budete muset použít název domény s jinou Azure AD.

Chcete-li odstranit vlastní název domény, musíte nejdřív zkontrolovat, že žádné prostředky ve vašem adresáři spoléhají na název domény. Název domény nelze odstranit z adresáře, pokud:

* Každý uživatel má uživatelské jméno, e-mailovou adresu nebo adresu proxy serveru, která obsahují název domény.
* Všechny skupiny má e-mailovou adresu nebo adresu proxy serveru, který zahrnuje název domény.
* Všechny aplikace ve službě Azure AD má aplikaci identifikátor ID URI, který zahrnuje název domény.

Musíte změnit nebo odstranit takových prostředků v adresáři služby Azure AD, chcete-li odstranit vlastní název domény.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Použijte PowerShell nebo rozhraní Graph API, které slouží ke správě názvů domén
Většinu úloh správy pro názvy domén v Azure Active Directory je možné dokončit také pomocí Microsoft PowerShell nebo programově pomocí rozhraní Azure AD Graph API.

* [Použití Powershellu ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Pomocí rozhraní Graph API ke správě názvů domén ve službě Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Další kroky
* [Další informace o názvech domén ve službě Azure AD](active-directory-add-domain-concepts.md)
* [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)

