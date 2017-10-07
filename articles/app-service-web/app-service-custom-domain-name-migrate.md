---
title: "aaaMigrate active DNS název tooAzure služby App Service | Microsoft Docs"
description: "Zjistěte, jak toomigrate vlastní název domény DNS, který je již přiřazen tooa live lokality tooAzure služby App Service bez odstávky."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
tags: top-support-issue
ms.assetid: 10da5b8a-1823-41a3-a2ff-a0717c2b5c2d
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: cephalin
ms.openlocfilehash: fbc4cc30dcb87efb2e867cb65c5404b667661e62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-an-active-dns-name-tooazure-app-service"></a>Migrace aktivní tooAzure název DNS služby App Service

Tento článek ukazuje, jak toomigrate active DNS název příliš[Azure App Service](../app-service/app-service-value-prop-what-is.md) bez odstávky.

Při migraci za provozu lokality a jeho DNS domény název tooApp služby, je tento název DNS obsluhující již provoz za provozu. Výpadek při překlad DNS během migrace hello vyhnete tak, že ho preventivně vazby hello active DNS název tooyour aplikaci služby App Service.

Pokud si nejste obavy o výpadek při překlad názvů DNS, najdete v části [mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md).

## <a name="prerequisites"></a>Požadavky

toocomplete tento postup:

- [Ujistěte se, že aplikaci aplikační služby není v úrovni FREE](app-service-web-tutorial-custom-domain.md#checkpricing).

## <a name="bind-hello-domain-name-preemptively"></a>Název domény hello ho preventivně vazby

Pokud jste ho preventivně vytvořit vazbu vlastní doménu, musíte provést obě následující hello před provedením jakýchkoli změn se záznamy DNS:

- Ověřit vlastnictví domény
- Povolit hello název domény pro vaši aplikaci.

Při migraci nakonec váš vlastní název DNS ze staré lokality toohello hello aplikaci služby App Service, budou v překlad DNS existovat nedojde k žádnému výpadku.

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-domain-verification-record"></a>Vytvořit záznam ověření domény

vlastnictví domény s tooverify, zaznamenejte přidat TXT. Hello záznam TXT mapuje z _awverify.&lt; subdoména >_ too_&lt;appname >. azurewebsites.net_. 

Hello záznam TXT, které potřebujete, závisí na hello chcete toomigrate záznam DNS. Příklady najdete v tématu hello následující tabulka (`@` obvykle představuje hello kořenové domény):  

| Příklad záznamu DNS | TXT hostitele | Hodnota TXT |
| - | - | - |
| @ (uživatel root) | _awverify_ | _&lt;AppName >. azurewebsites.net_ |
| WWW (pod) | _awverify.www_ | _&lt;AppName >. azurewebsites.net_ |
| \*(zástupný znak) | _awverify.\*_ | _&lt;AppName >. azurewebsites.net_ |

Poznamenejte si hello typ záznamu hello název DNS, které chcete toomigrate v stránku záznamů DNS. App Service podporuje mapování z CNAME a záznamy.

### <a name="enable-hello-domain-for-your-app"></a>Povolit hello domény pro vaši aplikaci.

V hello [portál Azure](https://portal.azure.com), v levém navigačním stránky aplikace hello hello, vyberte **vlastní domény**. 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

V hello **vlastní domény** stránky, vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ hello plně kvalifikovaný název domény přidali hello záznam TXT, jako například `www.contoso.com`. Pro doménu zástupných znaků (například \*. contoso.com), můžete použít libovolný název DNS, který odpovídá hello zástupné domény. 

Vyberte **ověření**.

Hello **přidat název hostitele** se aktivuje tlačítko. 

Ujistěte se, že **typ záznamu Hostname** je nastaven typ záznamu toohello DNS chcete toomigrate.

Vyberte **přidat název hostitele**.

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky. Aktualizujte data hello tooupdate hello prohlížeče.

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Váš vlastní název DNS je teď povolené v aplikaci Azure. 

## <a name="remap-hello-active-dns-name"></a>Přemapování název DNS služby active hello

Hello pouze jedinou levém toodo je přemapování vaše active tooApp toopoint záznamů DNS služby. Pravé teď stále odkazuje tooyour staré lokality.

<a name="info"></a>

### <a name="copy-hello-apps-ip-address-a-record-only"></a>Zkopírujte aplikace hello IP adresu (pouze záznam)

Pokud jsou přemapování záznam CNAME, tuto část přeskočte. 

záznam tooremap A, musíte aplikace App Service hello externí IP adresu, která se zobrazí v hello **vlastní domény** stránky.

Zavřít hello **přidat název hostitele** stránky tak, že vyberete **X** v pravém horním rohu hello. 

V hello **vlastní domény** stránky, zkopírujte aplikace hello IP adresu.

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

### <a name="update-hello-dns-record"></a>Aktualizace záznamu DNS hello

Zpět v hello stránky záznamů DNS vaší domény poskytovatele vyberte tooremap záznamů DNS hello.

Pro hello `contoso.com` kořen domény příklad, přemapovat hello záznam CNAME nebo A jako příklady hello v hello následující tabulka: 

| Příklad plně kvalifikovaný název domény | Typ záznamu | Hostitel | Hodnota |
| - | - | - | - |
| contoso.com (uživatel root) | A | `@` | IP adresu z [aplikace hello kopie IP adresu](#info) |
| www.contoso.com (pod) | CNAME | `www` | _&lt;AppName >. azurewebsites.net_ |
| \*. contoso.com (zástupný znak) | CNAME | _\*_ | _&lt;AppName >. azurewebsites.net_ |

Uložte nastavení.

Dotazy DNS by se měl spustit okamžitě po šíření záznamů DNS se stane řešení tooyour aplikaci služby App Service.

## <a name="next-steps"></a>Další kroky

Zjistěte, jak toobind vlastní SSL certifikát tooApp služby.

> [!div class="nextstepaction"]
> [Vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace](app-service-web-tutorial-custom-ssl.md)
