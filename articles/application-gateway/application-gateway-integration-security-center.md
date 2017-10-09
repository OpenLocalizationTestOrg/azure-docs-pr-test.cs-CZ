---
title: "aaaApplication brány integraci s Azure Security Center | Microsoft Docs"
description: "Tato stránka obsahuje informace o tom, jak je aplikační brána integrovaná do Azure Security Center."
documentationcenter: na
services: application-gateway
author: georgewallace
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
ms.author: gwallace
ms.openlocfilehash: 6f6ace105e84c01f525ab02938e81ce040c5c9d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a>Přehled integrace mezi aplikační brány a Azure Security Center

Zjistěte, jak aplikační brány a Security Center chránit prostředky vaší webové aplikace. Brány firewall webových aplikací aplikace brány (firewall webových aplikací) se integruje s [Security Center](../security-center/security-center-intro.md) tooprovide tooprevent bezproblémové zobrazení zjišťování a reakce toothreats toounprotected webových aplikací ve vašem prostředí.

## <a name="overview"></a>Přehled

Aplikace brány firewall webových aplikací je doporučeným v Centru zabezpečení pro ochranu před zneužitím a ohrožení zabezpečení webových aplikací. Web povoleno prostředky, které nejsou chráněné firewall webových aplikací se zobrazí v Centru zabezpečení hello jako doporučení vysokou závažností. Doporučení pro brány firewall webové aplikace se zobrazují na hello **přehled** v části **aplikace**.

![Integrace s security center][1]

Kliknutím na tlačítko všechna doporučení týkající se brány firewall webových aplikací, otevře se nové okno s podrobnostmi hello hello doporučení.

## <a name="add-a-web-application-firewall-tooan-existing-resource"></a>Přidání webové aplikace brány firewall tooan existující prostředky

Přejděte příliš**více služeb** > **zabezpečení a identita** > **Security Center** a na hello **Security Center – přehled**  okně klikněte na tlačítko **aplikace**. Na hello **Security Center – aplikace** okně hello tabulka obsahuje seznam aplikací, které Security Center zjistila v rámci vašeho předplatného.

![webové aplikace][3]

Kliknutím na webovou aplikaci s problémem důležité získat hello **stav zabezpečení aplikací** okno. V následující hello obrázek text hello webové aplikace, který není chráněný pomocí brány firewall webových aplikací. 

![webové prostředky, které nejsou chráněny.][2]

Klikněte na tlačítko **přidání brány firewall webových aplikací** pod **doporučení** tooopen hello **přidání brány Firewall webových aplikací** okno.

Pokud není máte existující aplikační bránu, nebo chcete toocreate nový, klikněte na tlačítko **vytvořit nový** a na hello **vytvoření nové brány Firewall webových aplikací** a klikněte na **Microsoft - Aplikační brána**. To vás provede kroky toocreate hello aplikační brány. Webové aplikace je nyní přidána jako chráněný prostředek, Security Center nyní sleduje tento prostředek je chráněn brány firewall webových aplikací. To se nepřidá jako člen fondu back-end.

Pokud máte existující aplikační brány, můžete ho **použít existující řešení**

![okno Přidání brány firewall webových aplikací][4]

Přidávání webové aplikace tooan Aplikační brána prostřednictvím Security Center nepřidá hello prostředků jako člen fondu back-end, musí provést na prostředku aplikační brány hello přímo.

## <a name="add-a-resource-tooan-existing-web-application-firewall"></a>Přidejte prostředků tooan existující brány firewall webových aplikací

Přejděte příliš**více služeb** > **zabezpečení a identita** > **Security Center** a na hello **Security Center – přehled**  okně klikněte na tlačítko **Partner solutions**. Zobrazit existující brány aplikace podporující Security Center v hello **partnerských řešení** okno.

![Partnerská řešení][7]

Klikněte na tlačítko **odkaz aplikace** tooopen hello **odkaz aplikace** okně tady budete mít hello možnosti tooselect stávající aplikace. Zvolte tooprotect hello aplikace a klikněte na **OK**. To nepřidá hello webové aplikace toohello back-end fondu hello aplikační brány. Toto nastaví hello prostředky jako chráněný prostředek, Security Center můžete sledovat jeho. tooadd hello zdroj jako člena fondu back-end, musí provést na hello aplikační bránu, v okně aktuální hello můžete kliknout na **řešení konzoly** toobe prováděné toohello prostředku aplikační brány, kde můžete přidat hello web fond back-end toohello aplikací.

![partnerských řešení aplikací][6]

## <a name="finalize-configuration"></a>Dokončení konfigurace

Security Center sleduje aplikace tooan aplikační brány přidat jako chráněného prostředku.  Sleduje stav hello tento prostředek a zajistí, že je chráněn aplikační brány. dalším krokem Hello je tooadd hello privátní IP, veřejnou IP adresu nebo síťové KARTĚ fondu back-end toohello virtuálního počítače brány aplikace hello. Dokud se to provádí další doporučení **dokončit ochranu aplikace** se nezobrazí, dokud se přidá hello prostředků.

![okno Přidání brány firewall webových aplikací][5]

## <a name="security-alerts"></a>Výstrahy zabezpečení

V rámci Security Center přejděte příliš**detekce** > **výstrahy zabezpečení**.  Zde můžete najít výstrahy firewall webových aplikací pro application Gateway. Výstrahy jsou rozdělené podle pravidla firewall webových aplikací.

![výstrahy zabezpečení][8]

Kliknutím na pravidlo poskytne seznam výstrah pro tuto konkrétní pravidlo firewall webových aplikací. Každá výstraha zobrazí další podrobnosti o vyhledání hello. Podrobnosti o Hello zadejte bránu odkaz toohello aplikace.
 
![Podrobnosti výstrahy][9]

## <a name="next-steps"></a>Další kroky

jak brány firewall webových aplikací tooenable na existující aplikační brány, navštivte toolearn [vytvoření nebo aktualizace služby Azure Application Gateway pomocí brány firewall webových aplikací](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png