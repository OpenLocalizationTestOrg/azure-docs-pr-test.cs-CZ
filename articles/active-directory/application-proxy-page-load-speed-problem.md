---
title: "aaaAn aplikace Proxy aplikací trvá příliš dlouho tooload | Microsoft Docs"
description: "Řešení potíží s výkonem načítání stránky s hello proxy aplikace služby Azure AD"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 4c7a51f96840966a1d88933fa4e30f39479d8a5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="an-application-proxy-application-takes-too-long-tooload"></a>Aplikace Proxy aplikací trvá příliš dlouho tooload

Tento článek vám pomůže toounderstand proč Azure AD Application Proxy aplikace může trvat dlouhou dobu tooload a co můžete provést tooresolve tento problém.

## <a name="overview"></a>Přehled
Pokud vaše aplikace pracují, ale zobrazí dlouho latence, může být některé dílčí vylepšení v topologii vaší sítě, které můžete zvážit rychlost tooimprove hello. Vyhodnocení topologiemi, najdete v části hello [sítě aspekty dokumentu](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Pokud nemáte vám tyto aspekty, bohužel mít nemáme aktuálně Další doporučení pro optimalizaci výkonu. Jako hello Proxy aplikace služby rozšíří toomore datových center, které mohou být blíže tooyou, může toosee vylepšené latence spustit přímo. centra toosee hello úplný seznam dat Azure, uvidíte hello [latence zkušební stránku](http://www.azurespeed.com/Azure/Latency). 

Hello datových centrech s hello Proxy aplikace služby lze najít pomocí hello [nástroj pro testování porty konektor](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Zpětná vazba týkající se umístění Proxy aplikací datových center 
Může být Azure datových center, které zatím neobsahují Proxy aplikace, ale povede tooa skvělé latence zlepšování za vás. Hello datového centra umístění v < aadapfeedback@microsoft.com > tak, jak jsme rozbalte jsme mohli používat tooplan vaší zpětné vazby.

Jsme práce na některé další možnosti, které pomáhají zlepšit latenci hello pro klienty, kteří aktuálně najdete v části dlouho latenci a být zda tooshare dokumentaci, jakmile je k dispozici.

## <a name="next-steps"></a>Další kroky
[Práce s existující místní proxy servery](application-proxy-working-with-proxy-servers.md)
