---
title: "Aplikace Azure Active Directory a hlavní objekty služeb | Microsoft Docs"
description: "Diskuzi o vztah mezi aplikací a hlavní objekty služby v Azure Active Directory"
documentationcenter: dev-center-name
author: dstrockis
manager: mbaldwin
services: active-directory
editor: 
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2016
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 4c75ade5f4e47ef64ccc0fe8af4b174c377dc7bc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Aplikace a služby hlavní objekty ve službě Azure Active Directory (Azure AD)
Někdy význam "aplikace" může být nesprávně pochopeny, pokud se používá v kontextu služby Azure AD. Cílem tohoto článku je zkontrolujte jasnější, o veškerém koncepční a konkrétní aspekty integrace aplikace Azure AD, s ilustraci registrace a souhlas pro [víceklientské aplikace](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Přehled
Aplikace, která byla integrována se službou Azure AD má důsledky, které jdou nad rámec aspekt softwaru. "Aplikace" se často používá jako koncepční termín, která odkazuje na nejen aplikačním softwaru, ale také jeho registrace Azure AD a role v ověřování/autorizace služby "konverzace" za běhu. Podle definice aplikace, můžou fungovat v [klienta](active-directory-dev-glossary.md#client-application) role (využívání prostředku), [server prostředků](active-directory-dev-glossary.md#resource-server) role (zpřístupňuje rozhraní API pro klienty), nebo dokonce i. Protokol konverzace je definované [toku OAuth 2.0 Authorization Grant](active-directory-dev-glossary.md#authorization-grant), povolení klienta nebo prostředků přístupu nebo ochrany zdroje dat v uvedeném pořadí. Nyní přejdeme na podrobnější úrovni a v tématu jak aplikačního modelu služby Azure AD, představuje v době návrhu a spuštění aplikace. 

## <a name="application-registration"></a>registrace aplikace
Když se zaregistrujete aplikaci v Azure AD [portál Azure][AZURE-Portal], jsou dva objekty vytvořené v klientovi Azure AD: objekt aplikace a objektu zabezpečení služby.

#### <a name="application-object"></a>objekt aplikace
Aplikaci Azure AD je definována pouze objekt aplikace, který se nachází v klientovi Azure AD, kde byla aplikace registrovaná, a jeho jedna označuje jako "home" klienta aplikace. Azure AD Graph [aplikace entity] [ AAD-Graph-App-Entity] definuje schéma vlastností objektu application. 

#### <a name="service-principal-object"></a>objekt zabezpečení služby
Objekt zabezpečení služby definuje zásady a oprávnění pro použití aplikace v konkrétních klienta, poskytuje základ pro objekt zabezpečení k reprezentaci aplikace za běhu. Azure AD Graph [ServicePrincipal entity] [ AAD-Graph-Sp-Entity] definuje schéma pro hlavní objekt služby vlastnosti. 

#### <a name="application-and-service-principal-relationship"></a>Aplikace a služby hlavní relace
Zvažte objekt aplikace jako *globální* reprezentace vaší aplikace pro použití napříč všech klientů a objekt služby, jako *místní* reprezentace pro použití v konkrétní klienta. Slouží objekt aplikace jako šablony, ze které běžné a výchozí vlastnosti jsou *odvozené* pro použití při vytváření objektů zabezpečení odpovídající služby. Objekt aplikace proto má relaci 1:1 s aplikací softwaru a 1:many relaci s odpovídající hlavní objekty služby.

Hlavní název služby musí být vytvořen v každého klienta, kde bude aplikace používat, mu umožní navázání identity pro přihlášení nebo přístup k prostředkům se zabezpečené klientem. Jednoho klienta aplikace budou mít pouze jeden objekt služby (v domácí klienta), obvykle vytvoří a dá souhlas pro použití při registraci aplikace. Víceklientské webovou aplikaci nebo API bude mít i instančního objektu vytvořeny v každého klienta kde uživatele z tohoto tenanta souhlasí s tím jeho použití.  

> [!NOTE]
> Všechny změny provedené u objektu aplikace, se projeví také v objekt zabezpečení služby, v aplikačním domácí klientovi pouze (klienta, kde byl zaregistrován). Pro víceklientské aplikace, změny do objektu application se neprojeví v žádné příjemce klienty služby hlavní objekty, dokud je odebrat přístup prostřednictvím [Panel přístupu aplikace](https://myapps.microsoft.com) a udělena znovu.
><br>  
> Všimněte si také, že jsou nativních aplikací registrován jako víceklientské ve výchozím nastavení.
> 
> 

## <a name="example"></a>Příklad
Následující diagram znázorňuje vztah mezi objekt aplikace a odpovídající službu hlavní objekty v rámci ukázkové aplikace víceklientské názvem aplikace **HR aplikace**. V tomto scénáři jsou tři klienty Azure AD: 

* **Adatum** -klienta používaných ve společnosti, který vyvinul **HR aplikace**
* **Contoso** -klienta používá organizaci Contoso, což je příjemce **HR aplikace**
* **Společnost Fabrikam** -klientské Fabrikam organizace, které také využívá používá **HR aplikace**

![Vztah mezi objektem aplikace a objektu zabezpečení služby](./media/active-directory-application-objects/application-objects-relationship.png)

Krok 1 v předchozím diagramu je proces vytváření aplikace a služby hlavní objekty v domácí klienta aplikace.

V kroku 2 Když správce společnosti Contoso a Fabrikam dokončení souhlasu, objekt zabezpečení služby v klientovi Azure AD své společnosti a přiřazeno oprávnění, která správce. Všimněte si také, že HR aplikace může být nakonfigurován nebo navržena k umožnění souhlasu uživatelům pro jednotlivé použití.

V kroku 3 klienti příjemce HR aplikace (Contoso a Fabrikam) každou mají své vlastní objekt zabezpečení služby. Každý představuje dá souhlas jejich použití instance aplikace za běhu, řídí oprávnění příslušného správce.

## <a name="next-steps"></a>Další kroky
Objekt aplikace aplikace je přístupný prostřednictvím rozhraní Azure AD Graph API [portál Azure] [ AZURE-Portal] editoru manifestu aplikace, nebo [rutin prostředí Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), jako reprezentován jeho OData [aplikace entity][AAD-Graph-App-Entity].

Objekt zabezpečení aplikace service je přístupný prostřednictvím rozhraní Azure AD Graph API nebo [rutin prostředí Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), reprezentovaná jeho OData [ServicePrincipal entity] [ AAD-Graph-Sp-Entity].

[Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) je užitečné pro dotazování aplikace a služby hlavní objekty.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
