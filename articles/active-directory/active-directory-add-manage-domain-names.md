---
title: "aaaManaging vlastních názvů domén v Azure Active Directory | Microsoft Docs"
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
ms.openlocfilehash: 4b6d06fecf3be0621be51c38a1330eafdc1b4d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Správa vlastních názvů domén v Azure Active Directory
Název domény může být důležité identifikátor pro mnoho directory prostředky, jako součást:

* Pro uživatele uživatelského jména nebo e-mailové adresy
* Hello adresy pro skupinu
* identifikátor ID URI aplikace Hello pro aplikaci

Název domény, který je již ověřit, že toobe vlastníkem hello adresáře, která obsahuje hello prostředků může obsahovat prostředků v Azure Active Directory (Azure AD). Globální správce můžete provádět úlohy správy domény ve službě Azure AD.

> [!IMPORTANT]
> Společnost Microsoft doporučuje, která můžete spravovat Azure AD pomocí hello [centra pro správu Azure AD](https://aad.portal.azure.com) v hello hello portál Azure místo použití portálu Azure classic, kterou se odkazuje v tomto článku. Jak toomanage vaše názvů domén v Centru pro správu hello Azure AD, najdete v části [Správa vlastních názvů domén v Azure Active Directory](active-directory-domains-manage-azure-portal.md).

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a>Nastavte název hello primární domény pro váš adresář Azure AD
Při vytváření adresáře hello počáteční název domény, jako je například 'contoso.onmicrosoft.com', je také název hello primární domény pro váš adresář. primární doménu Hello je hello výchozí název domény pro nového uživatele při vytváření nového uživatele v hello [portál Azure classic](https://manage.windowsazure.com/), nebo dalších portálech, jako je například portál pro správu hello Office 365. To zjednodušuje proces hello správce toocreate noví uživatelé portálu hello.

Název toochange hello primární domény pro váš adresář:

1. Přihlaste se toohello [portál Azure classic](https://manage.windowsazure.com/) pomocí uživatelského účtu, který je globálním správcem adresáře Azure AD.
2. Vyberte **služby Active Directory** na levém navigačním panelu hello.
3. Otevřete svůj adresář.
4. Vyberte hello **domén** kartě.
5. Vyberte hello **změnu primární** tlačítka na panelu příkazů hello.
6. Vyberte doménu hello chcete toobe hello nové primární domény pro váš adresář.

Můžete změnit název hello primární domény pro váš adresář toobe všechny ověřené vlastní doménu, která není federovaný. Změna hello primární domény pro váš adresář nedojde ke změně hello uživatelská jména pro všechny stávající uživatele.

## <a name="add-custom-domain-names-tooyour-azure-ad"></a>Přidat vlastní doménu názvy tooyour Azure AD
Můžete přidat až adresář Azure AD tooeach názvy vlastních domén too900. Hello proces příliš[, přidejte další vlastní domény název](active-directory-add-domain.md) hello stejné pro hello první vlastní název domény.

## <a name="add-subdomains-of-a-custom-domain"></a>Přidat subdomény vlastní domény
Pokud chcete tooadd názvu domény třetí úrovně, třeba directory tooyour 'europe.contoso.com', měli byste nejprve přidat a ověřit hello domény druhé úrovně, jako je například contoso.com. Hello subdomény bude automaticky ověřit pomocí služby Azure AD. ověření, toosee, který hello subdomény, kterou jste právě přidali, aktualizace hello stránku hello prohlížeč, který obsahuje seznam domén hello ve vašem adresáři.

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a>Jaké toodo, pokud změníte hello registrátora DNS pro vlastní název domény
Pokud změníte hello registrátora DNS pro vlastní název domény, můžete dál toouse vlastního názvu domény se službou Azure AD samotné bez přerušení a bez další konfigurace úlohy. Pokud používáte vlastní název domény pro Office 365, Intune nebo jiné služby, které jsou závislé na vlastních názvů domén ve službě Azure AD, naleznete v dokumentaci toohello pro tyto služby.

## <a name="delete-a-custom-domain-name"></a>Odstranit vlastní název domény
Vlastní název domény můžete odstranit ze služby Azure AD, pokud vaše organizace už používá název domény, nebo pokud potřebujete toouse název domény s jinou Azure AD.

toodelete vlastní název domény, musíte napřed zajistit, aby žádné prostředky ve vašem adresáři spoléhají na název domény hello. Název domény nelze odstranit z adresáře, pokud:

* Každý uživatel má uživatelské jméno, e-mailovou adresu nebo adresu proxy serveru, která obsahují název domény hello.
* Všechny skupiny má e-mailovou adresu nebo adresu proxy serveru, která zahrnuje název domény hello.
* Všechny aplikace ve službě Azure AD má aplikaci identifikátor ID URI, který zahrnuje název domény hello.

Musíte změnit nebo odstranit takových prostředků v adresáři služby Azure AD, než budete moct odstranit hello vlastní název domény.

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a>Pomocí prostředí PowerShell nebo rozhraní Graph API názvů domén toomanage
Většinu úloh správy pro názvy domén v Azure Active Directory je možné dokončit také pomocí Microsoft PowerShell nebo programově pomocí rozhraní Azure AD Graph API.

* [Pomocí názvů domén toomanage prostředí PowerShell ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Pomocí názvů domén toomanage rozhraní Graph API v Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Další kroky
* [Další informace o názvech domén ve službě Azure AD](active-directory-add-domain-concepts.md)
* [Správa vlastních názvů domén](active-directory-add-manage-domain-names.md)

