---
title: "aaaManaging vlastních názvů domén v Azure Active Directory | Microsoft Docs"
description: "Koncepty správy a postupy pro správu název domény v Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 5063cd0a-dba2-4ba9-aa65-b8117490d73a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: curtand;jeffsta
ms.openlocfilehash: 4719524c4e972f6c981db39f016729da13b37670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Správa vlastních názvů domén v Azure Active Directory
Název domény, je důležitou součástí hello identifikátoru pro mnoho prostředků adresáře: je součástí uživatelského jména nebo e-mailové adresy pro uživatele, část hello adresy pro skupinu a můžou být součástí identifikátor ID URI aplikace hello pro aplikaci. Název domény, který už je ověřený jako vlastníkem hello adresáře, která obsahuje hello prostředků může obsahovat prostředků v Azure Active Directory (Azure AD). Globální správce můžete provádět úlohy správy domény ve službě Azure AD.

## <a name="set-hello-primary-domain-name-for-your-azure-ad-directory"></a>Nastavte název hello primární domény pro váš adresář Azure AD
Při vytváření adresáře hello počáteční název domény, jako je například 'contoso.onmicrosoft.com', je také název primární doménu hello. Při vytváření nového uživatele, je primární doménu Hello hello výchozí název domény pro nového uživatele. Nastavení primární doménu zjednodušuje proces hello správce toocreate noví uživatelé portálu hello. název primární domény toochange hello:

1. Přihlaste se toohello [portál Azure](https://portal.azure.com) pomocí účtu, který je globálním správcem adresáře hello.
2. Vyberte **další služby**, zadejte **Azure Active Directory** v hello textového pole a pak vyberte **Enter**.
   
   ![Správa uživatelů otevírání](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Na hello ***název adresáře*** vyberte **názvy domén**.
4. Na hello  ***název adresáře* -názvy domén** okně, vyberte hello název domény, například název primární domény toomake hello.
5. Na hello ***název domény*** okno (to znamená, hello okno, které se otevře s nový název domény v hlavě hello), vyberte hello **nastavit jako primární** příkaz. Potvrďte volbu po zobrazení výzvy.
   
   ![Zkontrolujte název domény, primární](./media/active-directory-domains-manage-azure-portal/make-primary.png)

Můžete změnit název hello primární domény pro váš adresář toobe všechny ověřené vlastní doménu, která není federovaný. Změna hello primární domény pro váš adresář nedojde ke změně hello uživatelská jména pro všechny stávající uživatele.

## <a name="add-custom-domain-names-tooyour-azure-ad"></a>Přidat vlastní doménu názvy tooyour Azure AD
Můžete přidat až adresář Azure AD tooeach názvy vlastních domén too900. Hello proces příliš[, přidejte další vlastní domény název](add-custom-domain.md) hello stejné pro hello první vlastní název domény.

## <a name="add-subdomains-of-a-custom-domain"></a>Přidat subdomény vlastní domény
Pokud chcete tooadd názvu domény třetí úrovně, třeba directory tooyour 'europe.contoso.com', měli byste nejprve přidat a ověřit hello domény druhé úrovně, jako je například contoso.com. Hello subdomény bude automaticky ověřit pomocí služby Azure AD. ověření, toosee, který hello subdomény, kterou jste právě přidali, aktualizace hello stránku hello prohlížeč, který obsahuje seznam domén hello.

## <a name="what-toodo-if-you-change-hello-dns-registrar-for-your-custom-domain-name"></a>Jaké toodo, pokud změníte hello registrátora DNS pro vlastní název domény
Pokud změníte hello registrátora DNS pro vlastní název domény, můžete dál toouse vlastního názvu domény se službou Azure AD samotné bez přerušení a bez další konfigurace úlohy. Pokud používáte vlastní název domény pro Office 365, Intune nebo jiné služby, které jsou závislé na vlastních názvů domén ve službě Azure AD, naleznete v dokumentaci toohello pro tyto služby.

## <a name="delete-a-custom-domain-name"></a>Odstranit vlastní název domény
Vlastní název domény můžete odstranit ze služby Azure AD, pokud vaše organizace už používá název domény, nebo pokud potřebujete toouse název domény s jinou Azure AD.

toodelete vlastní název domény, musíte napřed zajistit, aby žádné prostředky ve vašem adresáři spoléhají na název domény hello. Název domény nelze odstranit z adresáře, pokud:

* Každý uživatel má uživatelské jméno, e-mailovou adresu nebo adresu proxy serveru, která zahrnuje název domény hello.
* Všechny skupiny má e-mailovou adresu nebo adresu proxy serveru, která zahrnuje název domény hello.
* Všechny aplikace ve službě Azure AD má aplikaci identifikátor ID URI, který zahrnuje název domény hello.

Musíte změnit nebo odstranit takových prostředků v adresáři služby Azure AD, než budete moct odstranit hello vlastní název domény.

## <a name="use-powershell-or-graph-api-toomanage-domain-names"></a>Pomocí prostředí PowerShell nebo rozhraní Graph API názvů domén toomanage
Většinu úloh správy pro názvy domén v Azure Active Directory je možné dokončit také pomocí Microsoft PowerShell nebo programově pomocí rozhraní Azure AD Graph API.

* [Pomocí názvů domén toomanage prostředí PowerShell ve službě Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Pomocí názvů domén toomanage rozhraní Graph API v Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Další kroky
* [Přidat vlastní názvy domén](add-custom-domain.md)

