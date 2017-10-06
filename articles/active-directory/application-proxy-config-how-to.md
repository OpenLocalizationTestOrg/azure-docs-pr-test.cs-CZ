---
title: aaaHow tooconfigure aplikaci Proxy aplikace | Microsoft Docs
description: "Zjistěte, jak toocreate aplikaci Proxy aplikace v několika jednoduchých kroků konfigurace"
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
ms.openlocfilehash: c64019098fc124e4fe10b8288830bcd2b7239d3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-an-application-proxy-application"></a>Jak tooconfigure aplikaci Proxy aplikace

Tento článek vám pomůže toounderstand jak tooconfigure aplikaci Proxy aplikace v rámci Azure AD tooexpose vaše místní aplikace toohello cloudové.

## <a name="recommended-documents"></a>Doporučené dokumenty 

toolearn o hello počáteční konfigurace a vytvoření aplikace Proxy aplikací prostřednictvím hello portál pro správu, postupujte podle hello [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Podrobnosti o konfiguraci konektorů najdete v tématu [povolení Proxy aplikace v portálu Azure hello](active-directory-application-proxy-enable.md).

Informace o nahrávání certifikátů a používání vlastní domény najdete v tématu [práce s vlastní domény v Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).

## <a name="create-hello-applicationsetting-hello-urls"></a>Vytvoření hello hello aplikace nebo nastavení adresy URL

Pokud postupujete podle hello kroky hello [publikování aplikací pomocí proxy aplikace služby Azure AD](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) dokumentace a jsou získávání k chybě při vytváření aplikace hello, najdete v části Podrobnosti o chybě hello informace a návrhy, jak toofix hello aplikace. Většina chybové zprávy, obsahuje navrhované opravu. tooavoid běžné chyby, zkontrolujte:

-   Jste přihlášeni jako správce s toocreate oprávnění aplikace Proxy aplikace

-   Interní adresa URL Hello je jedinečný

-   externí adresu URL Hello je jedinečný

-   Hello vycházejte adresy URL protokolu http nebo https a končit "/"

-   Adresa URL Hello by měl být název domény, nikoli IP adresu

Hello chybová zpráva by měla zobrazit v pravém horním rohu hello při vytvoření aplikace hello. Můžete také vybrat hello oznámení ikonu toosee hello chybové zprávy.

   ![Oznámení řádku](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Nakonfigurujte konektory nebo konektor skupiny

Pokud máte potíže s konfigurací aplikace z důvodu upozornění o hello konektory a konektor skupiny, najdete pokyny k povolení Proxy aplikace získáte informace o tom toodownload konektory. Pokud chcete, aby toolearn více informací o konektory, přečtěte si téma hello [konektory dokumentaci](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

Pokud vaše konektory jsou neaktivní, znamená to, že jsou služby nelze tooreach hello. Často je vzhledem k tomu, že všechny hello požadované porty nejsou otevřené. toosee seznam požadované porty, najdete v části předpoklady hello hello povolení Proxy aplikace dokumentaci.

## <a name="upload-certificates-for-custom-domains"></a>Nahrát certifikáty pro vlastní domény

Vlastní domény povolit toospecify hello doménu vaší externí adresy URL. vlastní domény toouse, potřebujete certifikát hello tooupload pro tuto doménu. Informace o používání vlastní domény a certifikáty, najdete v části [práce s vlastní domény v Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains). 

Pokud narazíte na problémy odeslání vašeho certifikátu, vyhledejte hello chybové zprávy hello portálu a další informace o hello potížím s certifikátem hello. Běžné problémy s certifikátem patří:

-   Vypršela platnost certifikátu

-   Certifikát je podepsaný svým držitelem

-   Certifikát chybí privátní klíč hello

zobrazení chybových zpráv Hello v hello pravém horním rohu od vás tooupload hello certifikátu. Můžete také vybrat hello oznámení ikonu toosee hello chybové zprávy.

   ![Oznámení řádku](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Další kroky
[Publikování aplikací pomocí proxy aplikace služby Azure AD](application-proxy-publish-azure-portal.md)
