---
title: "aaaCreate webové aplikace do App Service Environment v1"
description: "Zjistěte, jak toocreate webové aplikace a plány služby app v v1 App Service Environment"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 983ba055-e9e4-495a-9342-fd3708dcc9ac
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/11/2017
ms.author: ccompy
ms.openlocfilehash: 322ef344517c54247b102fb4920e35645986ef98
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-web-app-in-an-app-service-environment-v1"></a>Vytvořit webovou aplikaci App Service Environment v1

> [!NOTE]
> Tento článek je o hello App Service Environment v1.  Není k dispozici novější verze hello služby App Service Environment, je snazší toouse a běží na výkonnější infrastruktury. Další informace o nové verzi hello začínat hello toolearn [toohello Úvod App Service Environment](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Přehled
Tento kurz ukazuje, jak toocreate webové aplikace a plánů služby App Service v [App Service Environment v1](app-service-app-service-environment-intro.md) (App Service Environment). 

> [!NOTE]
> Pokud chcete toolearn jak toocreate webové aplikace, ale nemusí toodo do služby App Service Environment, najdete v části [vytvoření webové aplikace .NET](app-service-web-get-started-dotnet.md) nebo jeden z hello související kurzy pro jiných jazyků a rozhraní.
> 
> 

## <a name="prerequisites"></a>Požadavky
Tento kurz předpokládá, že jste vytvořili služby App Service Environment. Pokud jste to neudělali, ještě, přečtěte si téma [vytvoření služby App Service Environment](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Vytvoření webové aplikace
1. V hello [portálu Azure](https://portal.azure.com/), klikněte na tlačítko **nové > Web + mobilní > Webová aplikace**. 
   
    ![][1]
2. Vyberte své předplatné.  
   
    Pokud máte více předplatných Uvědomte si, že toocreate na aplikaci ve službě App Service Environment, je nutné toouse hello stejné předplatné, které jste použili při vytváření prostředí pro hello. 
3. Vyberte nebo vytvořte skupinu prostředků.
   
    *Skupiny prostředků* povolit toomanage můžete jako jednotku související s prostředky Azure a jsou užitečné při vytváření *řízení přístupu na základě role* (RBAC) pravidla pro vaše aplikace. Další informace najdete v tématu [přehled Azure Resource Manageru][ResourceGroups]. 
4. Vyberte nebo vytvořte plán služby App Service.
   
    *Plánů služby App Service* jsou spravované sady webové aplikace.  Obvykle po výběru ceny hello ceny požadované je použité toohello plán služby App Service místo toohello jednotlivými aplikacemi. V App Service Environment platíte za výpočetní hello instancí přidělené App Service Environment toohello místo mít uvedené s vaší ASP.  tooscale až hello počet instancí webové aplikace můžete škálovat hello instancích vašeho plánu služby App Service a ovlivní všechny aplikace hello webové aplikace v tomto plánu.  Některé funkce, jako je například sloty lokality nebo integrace virtuální sítě také mít omezení množství v plánu hello.  Další informace najdete v tématu [přehled plánů služby Azure App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)
   
    Hello plánů služby App Service ve vašem App Service Environment poznáte podle hello umístění, které je uvedeno v části název plánu hello.  
   
    ![][5]
   
    Pokud chcete toouse plán služby App Service, která již existuje ve službě App Service Environment, vyberte jej. Pokud chcete toocreate nový plán aplikační služby, najdete v následující části tohoto kurzu hello [vytvořit plán služby App Service ve službě App Service Environment](#createplan).
5. Zadejte název hello pro vaši webovou aplikaci a pak klikněte na tlačítko **vytvořit**. 
   
    Pokud vaše App Service Environment používá adresu URL hello externí virtuální IP adresy aplikace v App Service Environment: [*sitename*]. [[ *název služby App Service Environment*]. p.azurewebsites.net místo [*sitename*]. azurewebsites.net
   
    Pokud vaše App Service Environment používá adresu URL interní VIP pak hello aplikace, jsou App Service Environment: [*sitename*]. [[ *subdomény zadané během vytváření App Service Environment*]   
    Po výběru vaší ASP během vytváření App Service Environment uvidíte hello subdomény aktualizovat níže **název**

## <a name="createplan"></a>Vytvořit plán služby App Service
Když vytvoříte plán služby App Service ve službě App Service Environment, vaše volby pracovního procesu se liší, protože neexistují žádné sdílené pracovních procesů v App Service Environment.  zaměstnanci Hello máte toouse jsou hello ty, které byly přiděleny App Service Environment toohello správce hello.  To znamená že toocreate nový plán, potřebujete další pracovních procesů přidělených tooyour App Service Environment fondu pracovních procesů než hello celkový počet instancí pro toohave napříč všemi plánu již v tomto fondu pracovních procesů.  Pokud nemáte k dispozici dostatek pracovních procesů ve vaší App Service Environment pracovní fond toocreate plánu, je třeba toowork s vaší tooget správce App Service Environment, je přidán.

Další rozdíl s plány služby App Service hostované služby App Service Environment je nedostatek hello ceny výběr.  Pokud máte služby App Service Environment jsou platícího za výpočetní prostředky používané hello systému a nemají přidané poplatky za hello plány v tomto prostředí.  Když vytvoříte plán služby App Service je obvykle vybrat cenový plán, určující vaši fakturaci.  Služby App Service Environment je v podstatě do privátní umístění, kde můžete vytvořit obsah.  Platíte za hello prostředí a není toohost svůj obsah.

Hello následující pokyny ukazují, jak naplánovat toocreate App Service při vytváření webové aplikace, jak je popsáno v předchozí části kurzu hello hello.

1. Klikněte na tlačítko **vytvořit nový** v hello plán Výběr uživatelského rozhraní a zadejte název pro váš plán jako za normálních okolností byste mimo App Service Environment.
2. Vyberte, chcete toouse z vašeho výběru umístění app Service Environment hello.
   
    Protože služby App Service Environment je v podstatě umístění privátní nasazení, zobrazuje se v umístění. 
   
    ![][2]
   
    Po výběru App Service Environment ve výběru umístění hello aktualizace hello vytvoření plánu služby App Service uživatelského rozhraní.  Hello umístění nyní zobrazuje hello název hello systému App Service Environment a hello oblast je v a hello ceny plán výběr se nahradí výběr fondu pracovního procesu.  
   
    ![][3]

### <a name="selecting-a-worker-pool"></a>Výběr fondu pracovních procesů
Za normálních okolností ve službě Azure App Service a mimo služby App Service Environment, existují 3 výpočetní velikostí, které jsou k dispozici s výběrem hello vyhrazené cena plánu.  Podobným způsobem pro App Service Environment můžete definovat až too3 fondy pracovních procesů a zadejte velikost výpočetních hello, který se používá pro tento fond pracovních procesů.  Co to znamená pro klienty hello App Service Environment je, že místo výběru cenový plán s velikostí výpočetní pro plán služby App Service, vyberete co se nazývá *fondu pracovních procesů*.  

Hello pracovní fond Výběr uživatelského rozhraní zobrazuje velikost výpočetních hello používaných pro tento fond pracovních procesů pod názvem hello.  Hello množství, které jsou k dispozici odkazuje toohow mnoho výpočetní instance jsou k dispozici pro použití v tomto fondu.  Hello celkového fondu, může mít více instancí než tento počet, ale tato hodnota se odkazuje toosimply kolik se nepoužívají.  Pokud potřebujete tooadjust vaší služby App Service Environment tooadd více výpočetní, najdete v části prostředky [konfigurace služby App Service Environment](app-service-web-configure-an-app-service-environment.md).

![][4]

V tomto příkladu najdete v části k dispozici pouze dva fondy pracovních procesů. Je to způsobeno hello App Service Environment správce pouze přidělené hostitele do těchto fondů dvě pracovního procesu.  Hello třetí se zobrazí po přidělené do ní virtuální počítače.  

## <a name="after-web-app-creation"></a>Po vytvoření webové aplikace
Existuje několik důležité informace týkající se spouštění webových aplikací a správu plány služby App Service v App Service Environment, potřebujete toobe vzít v úvahu.  

Jak již bylo uvedeno dříve, hello vlastník hello App Service Environment je zodpovědný za hello velikost hello systému a v důsledku odpovídají také pro zajištění, že je dostatečná kapacita toohost hello potřeby plány služby App Service. Pokud nejsou žádné dostupné pracovní procesy, které není možné toocreate se plán služby App Service.  Toto je také true tooscaling až vaše webová aplikace.  Pokud potřebujete více instancí, pak byste měli tooget tooadd správce vaší služby App Service Environment další pracovní procesy.

Po vytvoření webové aplikace a plán služby App Service je vhodné tooscale ho nahoru.  V App Service Environment musíte vždycky toohave minimálně 2 instance vaší služby App Service plán tooprovide odolnost proti chybám pro vaše aplikace.  Škálování App Service plán App Service Environment je hello stejné jako normální prostřednictvím hello plán služby App Service uživatelského rozhraní.  Další informace o škálování [jak tooscale webové aplikace ve službě App Service Environment](app-service-web-scale-a-web-app-in-an-app-service-environment.md)

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: ../azure-resource-manager/resource-group-overview.md
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
