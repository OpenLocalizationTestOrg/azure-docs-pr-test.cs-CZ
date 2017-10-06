---
title: "vytváření aplikací Proxy aplikace aaaProblem | Microsoft Docs"
description: "Jak tootroubleshoot problémy vytváření aplikace Proxy aplikace v portálu pro správu Azure AD hello"
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
ms.openlocfilehash: 24fab83c38a49a9e5754854acf2f9711e374e559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="problem-creating-an-application-proxy-application"></a>Problém s vytvořením aplikace Proxy aplikace 

Níže jsou uvedeny některé běžné problémy, hello uživatelé setkávají při vytváření nové aplikace proxy aplikace.

## <a name="recommended-documents"></a>Doporučené dokumenty 

toolearn Další informace o vytváření aplikací Proxy aplikací prostřednictvím hello portál pro správu, najdete v části [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Pokud jsou následující hello kroky v tomto dokumentu a dochází k výskytu k chybě při vytváření aplikace hello, podívejte se na podrobnosti o chybě hello informace a návrhy, jak toofix hello aplikace. Většina chybové zprávy, obsahuje navrhované opravu. 

## <a name="specific-things-toocheck"></a>Toocheck specifické položky

tooavoid běžné chyby, zkontrolujte:

-   Jste přihlášeni jako správce s toocreate oprávnění aplikace Proxy aplikace

-   Interní adresa URL Hello je jedinečný

-   externí adresu URL Hello je jedinečný

-   Hello vycházejte adresy URL protokolu http nebo https a končit "/"

-   Adresa URL Hello by měl být název domény a ne IP adresy

Hello chybová zpráva by měla zobrazit v pravém horním rohu hello při vytvoření aplikace hello. Můžete také vybrat hello oznámení ikonu toosee hello chybové zprávy.

   ![Oznámení řádku](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Další kroky
[Povolení Proxy aplikace v hello portálu Azure](active-directory-application-proxy-enable.md)
