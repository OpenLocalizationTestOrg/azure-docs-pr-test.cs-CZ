---
title: "Integrace brány aplikací s Azure Security Center | Microsoft Docs"
description: "Tato stránka obsahuje informace o tom, jak je aplikační brána integrovaná do Azure Security Center."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: davidmu
ms.openlocfilehash: abed4095bcacac4ca3995acfaafae2f16d5f1532
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Přehled integrace mezi aplikační brány a Azure Security Center

Zjistěte, jak aplikační brány a Security Center chránit prostředky vaší webové aplikace. Brány firewall webových aplikací aplikace brány (firewall webových aplikací) se integruje s [Security Center](../security-center/security-center-intro.md) zajistit bezproblémovou zobrazení, aby se zabránilo zjistit a reagovat na hrozby do nechráněných webových aplikací ve vašem prostředí.

## <a name="overview"></a>Přehled

Aplikace brány firewall webových aplikací je doporučeným v Centru zabezpečení pro ochranu před zneužitím a ohrožení zabezpečení webových aplikací. Web povoleno prostředky, které nejsou chráněné firewall webových aplikací se zobrazí v Centru zabezpečení jako doporučení vysokou závažností. Doporučení pro brány firewall webové aplikace se zobrazí na **přehled** v části **aplikace**.

![Integrace s security center][1]

Kliknutím na tlačítko všechna doporučení, otevře se nové okno s podrobnostmi o doporučení týkající brány firewall webových aplikací.

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a>Přidání brány firewall webových aplikací do stávajícího prostředku

Přejděte na **další služby** > **zabezpečení a identita** > **Security Center** a na **Security Center – přehled**  okně klikněte na tlačítko **aplikace**. Na **Security Center – aplikace** okně Tabulka obsahuje seznam aplikací, které Security Center zjistila v rámci vašeho předplatného.

![webové aplikace][3]

Kliknutím na webovou aplikaci s kritický problém, můžete získat **stav zabezpečení aplikací** okno. Na obrázku níže, webové aplikace, který není chráněný pomocí brány firewall webových aplikací. 

![webové prostředky, které nejsou chráněny.][2]

Klikněte na tlačítko **přidání brány firewall webových aplikací** pod **doporučení** otevřete **přidání brány Firewall webových aplikací** okno.

Pokud není máte existující aplikační bránu, nebo chcete vytvořit nový, klikněte na tlačítko **vytvořit nový** a na **vytvoření nové brány Firewall webových aplikací** a klikněte na **Microsoft – aplikace Brána**. To vás provede kroky k vytvoření aplikační brány. Webové aplikace je nyní přidána jako chráněný prostředek, Security Center nyní sleduje tento prostředek je chráněn brány firewall webových aplikací. To se nepřidá jako člen fondu back-end.

Pokud máte existující aplikační brány, můžete ho **použít existující řešení**

![okno Přidání brány firewall webových aplikací][4]

Přidání webové aplikace do služby application gateway pomocí Security Center nepřidá prostředek jako člen fondu back-end, musí provést na prostředek služby application gateway přímo.

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a>Přidání prostředku existující brány firewall webových aplikací

Přejděte na **další služby** > **zabezpečení a identita** > **Security Center** a na **Security Center – přehled**  okně klikněte na tlačítko **Partner solutions**. V zobrazovat existující brány aplikace podporující Security Center **partnerských řešení** okno.

![Partnerská řešení][7]

Klikněte na tlačítko **odkaz aplikace** otevřete **odkaz aplikace** okně zde budete mít možnost vybrat stávající aplikace. Vyberte aplikace, které chcete chránit a klikněte na tlačítko **OK**. To nepřidává žádné další webové aplikace na fond back-end službu application gateway. Toto nastaví prostředky jako chráněný prostředek, Security Center můžete sledovat jeho. Pokud chcete přidat jako člena fondu back-end prostředku, musí provést ve službě application gateway, v okně aktuální, můžete kliknout na **řešení konzoly** potřeba provést k prostředku aplikační brány, kde můžete přidat webovou aplikaci na fond back-end.

![partnerských řešení aplikací][6]

## <a name="finalize-configuration"></a>Dokončení konfigurace

Security Center zjišťuje aplikace, které jsou přidány do služby application gateway jako chráněný prostředek.  Sleduje stav tohoto prostředku a zajistí, že je chráněn aplikační brány. Dalším krokem je přidání privátní IP, veřejnou IP adresu nebo síťový adaptér virtuálního počítače do fondu back-end aplikační brány. Dokud se to provádí další doporučení **dokončit ochranu aplikace** se nezobrazí, dokud se přidá k prostředku.

![okno Přidání brány firewall webových aplikací][5]

## <a name="security-alerts"></a>Výstrahy zabezpečení

Přechod na v Security Center **detekce** > **výstrahy zabezpečení**.  Zde můžete najít výstrahy firewall webových aplikací pro application Gateway. Výstrahy jsou rozdělené podle pravidla firewall webových aplikací.

![výstrahy zabezpečení][8]

Kliknutím na pravidlo poskytne seznam výstrah pro tuto konkrétní pravidlo firewall webových aplikací. Každá výstraha zobrazí další podrobnosti o zjištění. Podrobnosti zadejte odkaz na službu application gateway.
 
![Podrobnosti výstrahy][9]

## <a name="next-steps"></a>Další kroky

Informace o povolení brány firewall webových aplikací na existující aplikační brány, najdete [vytvoření nebo aktualizace služby Azure Application Gateway pomocí brány firewall webových aplikací](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png