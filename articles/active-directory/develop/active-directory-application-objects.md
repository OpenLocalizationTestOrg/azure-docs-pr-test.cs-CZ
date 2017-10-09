---
title: "aaaAzure aplikace Active Directory a hlavní objekty služeb | Microsoft Docs"
description: "Diskuzi o hello vztah mezi aplikací a hlavní objekty služby v Azure Active Directory"
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
ms.openlocfilehash: ff7e308c0b326c3a32b101b7b323f2c0362763e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Aplikace a služby hlavní objekty ve službě Azure Active Directory (Azure AD)
Někdy hello význam hello pojem "aplikace" může být nesprávně pochopeny, pokud se používá v kontextu hello Azure AD. Hello cílem tohoto článku je toomake ho jasnější o veškerém koncepční a konkrétní aspekty integrace aplikace Azure AD, se registrace a souhlas pro ilustraci [víceklientské aplikace](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Přehled
Aplikace, která byla integrována se službou Azure AD má důsledky, které jdou nad rámec aspekt softwaru hello. "Aplikace" se často používá jako koncepční termín, odkazující toonot pouze hello hello aplikačním softwaru, ale také jeho registrace Azure AD a role v ověřování/autorizace služby "konverzace" za běhu. Podle definice aplikace, můžou fungovat v [klienta](active-directory-dev-glossary.md#client-application) role (využívání prostředku), [server prostředků](active-directory-dev-glossary.md#resource-server) role (tooclients zpřístupňuje rozhraní API) nebo dokonce i. Hello konverzace protokolu jsou určené [toku OAuth 2.0 Authorization Grant](active-directory-dev-glossary.md#authorization-grant), povolení hello klienta nebo prostředků tooaccess nebo chránit zdroje dat v uvedeném pořadí. Nyní přejdeme na podrobnější úrovni a v tématu Jak hello Azure AD aplikační model představuje v době návrhu a spuštění aplikace. 

## <a name="application-registration"></a>registrace aplikace
Když se zaregistrujete aplikaci Azure AD v hello [portál Azure][AZURE-Portal], jsou dva objekty vytvořené v klientovi Azure AD: objekt aplikace a objektu zabezpečení služby.

#### <a name="application-object"></a>objekt aplikace
Aplikaci Azure AD je definována pouze objekt aplikace, který se nachází v klientovi hello Azure AD, kde byla aplikace hello zaregistrovaná, a jeho jedna označuje jako "home" klienta aplikace hello. Hello Azure AD Graph [aplikace entity] [ AAD-Graph-App-Entity] definuje schéma hello vlastností objektu application. 

#### <a name="service-principal-object"></a>objekt zabezpečení služby
objekt zabezpečení služby Hello definuje hello zásady a oprávnění pro použití aplikace v konkrétních klienta, poskytuje základ hello aplikace hello hlavní toorepresent zabezpečení při spuštění. Hello Azure AD Graph [ServicePrincipal entity] [ AAD-Graph-Sp-Entity] definuje schéma hello vlastností Hlavní objekt služby. 

#### <a name="application-and-service-principal-relationship"></a>Aplikace a služby hlavní relace
Vezměte v úvahu objekt aplikace hello jako hello *globální* reprezentace vaší aplikace pro použití ve všech klientů a hello instanční objekt jako hello *místní* reprezentace pro použití v konkrétní klient. slouží jako hello šablony, ze které běžné a výchozí vlastnosti jsou zprostředkovatele Hello objektu application *odvozené* pro použití při vytváření objektů zabezpečení odpovídající služby. Objekt aplikace proto má relaci 1:1 s hello softwarová aplikace a 1:many relaci s odpovídající hlavní objekty služby.

Hlavní název služby musí být vytvořeny v každém klientovi, kde aplikace hello se použije, jeho povolení tooestablish identity přihlášení nebo tooresources přístup se zabezpečeny hello klienta. Jednoho klienta aplikace budou mít pouze jeden objekt služby (v domácí klienta), obvykle vytvoří a dá souhlas pro použití při registraci aplikace. Víceklientské webovou aplikaci nebo API bude mít i instančního objektu vytvořeny v každého klienta kde uživatele z tohoto tenanta souhlasí tooits použití.  

> [!NOTE]
> Všechny změny objektu application tooyour, se projeví také v objekt zabezpečení služby, v aplikaci hello domácí klientovi pouze (kde byl zaregistrován hello klienta). Pro více klientů aplikace objektu application toohello změny se neprojeví v žádné příjemce klienty služby hlavní objekty, dokud se neodstraní hello přístup prostřednictvím hello [Panel přístupu aplikace](https://myapps.microsoft.com) a udělena znovu.
><br>  
> Všimněte si také, že jsou nativních aplikací registrován jako víceklientské ve výchozím nastavení.
> 
> 

## <a name="example"></a>Příklad
Hello následující diagram znázorňuje hello vztah mezi objektem aplikace a odpovídající službu hlavní objekty v kontextu hello vzorové víceklientské aplikace názvem aplikace **HR aplikace**. V tomto scénáři jsou tři klienty Azure AD: 

* **Adatum** – hello klienta používaného hello společnosti, který vyvinul hello **HR aplikace**
* **Contoso** – hello klienta používaného hello organizaci Contoso, který je příjemce hello **HR aplikace**
* **Společnost Fabrikam** – hello klienta používaného hello Fabrikam organizace, které také využívá hello **HR aplikace**

![Vztah mezi objektem aplikace a objektu zabezpečení služby](./media/active-directory-application-objects/application-objects-relationship.png)

V předchozím diagramu hello krok 1 je hello proces vytváření aplikace hello a hlavní objekty služby v domácí klienta aplikace hello.

V kroku 2 po dokončení souhlasu, Contoso a Fabrikam správci objekt zabezpečení služby se vytvoří v klientovi Azure AD své společnosti a oprávnění přiřazené hello tento správce hello udělena. Všimněte si také, že tuto aplikaci hello HR může být nakonfigurován určené tooallow souhlasu uživatelům pro jednotlivé použití.

V kroku 3 hello příjemce klienty hello HR aplikace (Contoso a Fabrikam) každou mají své vlastní objekt zabezpečení služby. Každý představuje jejich použití instance hello aplikace za běhu, řídí, které se dá souhlas oprávnění hello hello příslušného správce.

## <a name="next-steps"></a>Další kroky
Objekt aplikace aplikace je přístupná prostřednictvím hello Azure AD Graph API hello [portál Azure] [ AZURE-Portal] editoru manifestu aplikace, nebo [rutin prostředí Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), reprezentovaná jeho OData [aplikace entity][AAD-Graph-App-Entity].

Objekt zabezpečení aplikace service je přístupná prostřednictvím hello Azure AD Graph API nebo [rutin prostředí Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0), reprezentovaná jeho OData [ServicePrincipal entity] [ AAD-Graph-Sp-Entity].

Hello [Azure AD Graph Explorer](https://graphexplorer.azurewebsites.net/) je užitečné pro dotazování hello aplikace a služby hlavní objekty.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com
