---
title: "aaaIntro tooauthentication ve službě Azure Automation | Microsoft Docs"
description: "Tento článek obsahuje přehled zabezpečení automatizace a hello různé metody ověřování k dispozici pro účty Automation ve službě Azure Automation."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: automation security, secure automation; automation authentication
ms.assetid: 4a6bc2f5-c5a2-4dfb-b10d-7950d750dee8
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2017
ms.author: magoedte
ROBOTS: NOINDEX
ms.openlocfilehash: 4b4409b5be010c16f7bf00a9a0f617e3617d4663
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooauthentication-in-azure-automation"></a>Úvod tooauthentication ve službě Azure Automation  
Automatizace Azure umožňuje tooautomate úlohy s prostředky v Azure, místně a u jiných poskytovatelů cloudu, například Amazon Web Services (AWS).  Aby runbook tooperform požadované akce, musí mít oprávnění toosecurely přístup k hello prostředkům s minimálními požadovanými v rámci předplatného hello právy hello.

Tento článek se zabývá hello podporují různé scénáře ověřování ve službě Azure Automation a zobrazit, jak začít tooget na základě hello prostředí nebo prostředí budete potřebovat toomanage.  

## <a name="automation-account-overview"></a>Přehled účtu Automation
Při spuštění služby Azure Automation pro hello poprvé, musíte vytvořit alespoň jeden účet Automation. Účty Automation umožňují tooisolate vaše prostředky Automation (runbooky, prostředků, konfigurace) od hello prostředky obsažené v jiných účtech Automation. Můžete vytvořit prostředky tooseparate účty Automation do samostatných logických prostředí. Jeden účet můžete například použít pro vývoj, druhý k produkci a další pro svoje místní prostředí.  Účet Azure Automation se liší od účtu Microsoft a účtů vytvořených v rámci vašeho předplatného Azure.

Hello prostředky Automation jednotlivých účtů Automation jsou přidružené k jedné oblasti Azure, ale účty Automation můžou spravovat všechny prostředky hello ve vašem předplatném. Hello hlavním důvodem toocreate účty Automation v různých oblastech by, pokud máte zásady, které vyžadují datům a prostředkům toobe izolované tooa určité oblasti.

> [!NOTE]
> Účty Automation a hello prostředky, které obsahují jsou vytvořené v hello portálu Azure, nelze získat přístup v hello portál Azure classic. Pokud chcete toomanage tyto účty nebo jejich prostředky pomocí prostředí Windows PowerShell, je nutné použít hello moduly Azure Resource Manageru.
>

Všechny hello úlohy, které můžete provádět proti prostředků pomocí Azure Resource Manager a hello rutin Azure ve službě Azure Automation musí ověřit tooAzure použití ověřování na základě přihlašovacích údajů organizační identity Azure Active Directory.  Ověřování pomocí certifikátů bylo původní metodou ověřování hello v režimu Azure Service Management, ale byl složitý toosetup.  Ověřování tooAzure pomocí uživatele Azure AD bylo zavedeno v 2014 toonot pouze zjednodušit tooconfigure proces hello účet pro ověřování, ale také možnost podpory hello toonon interaktivně ověření tooAzure pomocí jediného uživatelského účtu, který odpracoval s Azure Resource Manager i klasické prostředky.   

Teď, když vytvoříte nový účet služby Automation v hello portálu Azure, automaticky vytvoří:

* Účet Spustit jako, který vytvoří nový objekt služby v Azure Active Directory, certifikát a přiřadí hello Přispěvatel přístupu na základě role řízení (RBAC), který bude použit toomanage prostředky Resource Manageru pomocí sad runbook.
* Classic účet Spustit jako tím, že nahrajete certifikát správy, který bude možné použít toomanage Azure Service Management nebo klasické prostředky pomocí runbooků.  

Řízení přístupu na základě role je k dispozici s Azure Resource Manager toogrant povolené akce tooan Azure AD uživatelský účet a účet Spustit jako a ověřování takového objektu služby.  Přečtěte si prosím [řízení přístupu na základě Role v Azure Automation článku](automation-role-based-access-control.md) pro další informace o toohelp vývojem vašeho modelu pro správu oprávnění automatizace.  

Runbook spuštěných v Hybrid Runbook Worker ve vašem datovém centru nebo s výpočetními službami v AWS nelze použít hello stejné metody, která se obvykle používá pro ověřování tooAzure prostředky sad runbook.  Je to proto, že tyto prostředky jsou spuštěné mimo Azure a budou proto vyžadovat svoje vlastní zabezpečovací přihlašovací údaje definované v tooresources tooauthenticate automatizace, které budou přistupovat místně.  

## <a name="authentication-methods"></a>Metody ověřování
Hello následující tabulka shrnuje hello různé metody ověřování pro jednotlivá prostředí podporovaná službou Azure Automation a hello článek popisující, jak toosetup ověřování runbooků.

| Metoda | Prostředí | Článek |
| --- | --- | --- |
| Uživatelský účet Azure AD |Azure Resource Manager a správa služby Azure |[Ověření runbooků pomocí uživatelského účtu Azure AD](automation-create-aduser-account.md) |
| Azure spuštěné jako účet |Azure Resource Manager |[Ověření runbooků pomocí účtu Spustit v Azure jako](automation-sec-configure-azure-runas-account.md) |
| Azure Classic spuštěné jako účet |Azure Service Management |[Ověření runbooků pomocí účtu Spustit v Azure jako](automation-sec-configure-azure-runas-account.md) |
| Ověřování systému Windows |Místní datové centrum |[Ověření runbooků pro proces Hybrid Runbook Worker](automation-hybrid-runbook-worker.md) |
| Přihlašovací údaje služby Amazon Web Services |Amazon Web Services |[Ověření runbooků pomocí Amazon Web Services (AWS)](automation-config-aws-account.md) |
