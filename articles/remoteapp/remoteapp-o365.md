---
title: aaaUsing Office s Azure Remoteappem | Microsoft Docs
description: "Zjistěte, jak spolupracují Office s Azure Remoteappem"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: f1773baf-8aa1-423c-a2f9-e4679e0463d3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d065c1a0a2191c9e2e405264a835cecf791d6ab7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="using-office-with-azure-remoteapp"></a>Používání Office s Azure Remoteappem
> [!IMPORTANT]
> Azure RemoteApp se přestává používat dne 31. srpna 2017. Čtení hello [oznámení](https://go.microsoft.com/fwlink/?linkid=821148) podrobnosti.
> 
> 

Máte dvě možnosti pro hostování aplikací Office v Azure Remoteappu: Office 365 ProPlus nebo Office 2013 Professional Plus zkušební verze.

**Hey víte, že máme nové, lepší článek, který nahradí brzy to? Podívejte se na [jak toouse předplatné služeb Office 365 s Azure Remoteappem](remoteapp-officesubscription.md). Pokrývá všechny hello údaje, které potřebujete k používání Office 365 + Azure RemoteApp.**

## <a name="office-365-proplus"></a>Office 365 ProPlus
Můžete vytvořit kolekci RemoteApp pomocí image šablony Office 365 ProPlus hello. Tato možnost vám umožňuje tooextend vaší tooRemoteApp služby Office 365. Musí mít existujícího plánu předplatného a uživatelé musí mít licenci pro hello služby Office 365 ProPlus, buď samostatně nebo prostřednictvím plány služeb Office 365 hello.

Vzdálené aplikace RemoteApp podporuje aktivaci sdílené počítače aplikace Office 365. Při povolení sdílené počítače aktivace a používání hello [nástroj pro nasazení Office](http://www.microsoft.com/download/details.aspx?id=36778) pro instalaci, Office 365 ProPlus nainstaluje bez aktivace. Když uživatel se přihlásí do kolekce, která obsahuje Office 365, Office zkontroluje toosee, pokud uživatel hello zřízená pro Office 365 ProPlus. Pokud ano, se Office dočasně aktivuje Office 365 ProPlus – tato Aktivace trvá až do této uživatelé přihlásí mimo provoz hello.

toouse aktivace sdílené počítače aplikace Office 365, je nutné toocreate [vlastní šablony](remoteapp-create-custom-image.md) a instalaci Office 365 ProPlus existuje, následující [těchto pokynů](https://technet.microsoft.com/library/dn782858.aspx).

Můžete spravovat vaši uživatelé Office 365 licence na hello [portálu pro správu Office 365](https://portal.office365.com/). Přečtěte si další informace o [plány služeb Office 365](http://technet.microsoft.com/library/office-365-plan-options.aspx).  

## <a name="office-2013-professional-plus-trial"></a>Zkušební verzi Office 2013 Professional Plus
Během 30denní zkušební období služby RemoteApp můžete použít hello Office 2013 Professional Plus (zkušební verze) šablony image toocreate kolekce vzdálené aplikace RemoteApp. Můžete přiřadit uživatele toothis zkušební kolekce pomocí jejich pracovních účtů Azure Active Directory nebo účty Microsoft. Je potřeba žádné další předplatné.

Toto je skvělou možnost tookick hello otestovat a získat dobrý pocit pro Office v Remoteappu. Tato možnost je však určený pro vyhodnocování a testování pouze. Kolekce vzdálené aplikace RemoteApp vytvořené pomocí image šablony Office 2013 Professional Plus (zkušební verze) hello nemůže být přešla tooproduction režimu a na konci hello hello zkušebního období bude zakázán.

## <a name="switching-from-trial-tooproduction"></a>Přepínání z zkušební tooproduction
Když začnete používat 30denní bezplatnou zkušební verzi, poznámky v hello vzdálené aplikace RemoteApp části portálu hello zjistit, jak dlouho mají left v zkušební verze hello předtím, než je nutné tootransition tooa placené účtu. Můžete si aktivovat tooproduction režimu vaší účet a přepínač pomocí odkazu hello v této poznámky.

Po aktivaci účtu, tato akce ovlivní všechny kolekce vzdálené aplikace RemoteApp hello ve vašem účtu.

* Kolekce, které jsou spuštěny s hello Windows Server 2012 R2 nebo Image šablony Office 365 ProPlus hello přejde tooproduction bezproblémově. Všechna uživatelská data a nastavení, včetně probíhající relace, zůstanou beze změn.
* Pokud jste nahráli Image vlastní šablony, pomocí těchto bitových kopií kolekce bude také přechod bezproblémově.
* image šablony Office 2013 Professional Plus (zkušební verze) Hello slouží pouze zkušební verze. Kolekce s této image šablony nemůže být přešla tooproduction. Bude potřeba seřadit ve stavu "Zakázat".

Pokud tooproduction režimu není přechod pomocí hello vypršení platnosti zkušební verze, bude vaše kolekce vzdálené aplikace RemoteApp zakázané. Nemusíte si dělat starosti – nastavení a data uživatelů se ukládají pro jinou 90 dnů, takže lze aktivovat tooproduction režimu bez ztráty dat. vaší služby a přepínače.

