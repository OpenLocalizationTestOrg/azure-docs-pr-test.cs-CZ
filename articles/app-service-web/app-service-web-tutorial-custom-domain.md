---
title: "aaaMap existující vlastní DNS název webové aplikace tooAzure | Microsoft Docs"
description: "Zjistěte, jak tooadd existující vlastní domény DNS název webové aplikace tooa (jednoduché domény), back-end mobilní aplikace nebo aplikace API v Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2c4eea3c56c758ca11355554321ffa52dd2c6b9d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="map-an-existing-custom-dns-name-tooazure-web-apps"></a>Mapovat existující vlastní DNS název tooAzure webové aplikace

[Azure Web Apps](app-service-web-overview.md) je vysoce škálovatelná služba s automatickými opravami pro hostování webů. Tento kurz ukazuje, jak toomap existující vlastní DNS název tooAzure webové aplikace.

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Mapování subdoména (například `www.contoso.com`) pomocí záznamu CNAME
> * Mapování kořenové domény (například `contoso.com`) pomocí záznamu a.
> * Namapovat doménu zástupných znaků (například `*.contoso.com`) pomocí záznamu CNAME
> * Automatizovat mapování domény pomocí skriptů

Můžete použít buď **záznam CNAME** nebo **záznam** toomap vlastní DNS název tooApp služby. 

> [!NOTE]
> Doporučujeme použít záznam CNAME pro všechny vlastní názvy DNS kromě kořenové domény (například `contoso.com`).

toomigrate živý web a jeho DNS domény název tooApp služby, najdete v části [migrovat active tooAzure název DNS služby App Service](app-service-custom-domain-name-migrate.md).

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

* [Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.
* Zakupte název domény a ujistěte se, že máte přístup toohello DNS registru pro poskytovatele domény (například GoDaddy).

  Tooadd záznamy DNS, třeba pro `contoso.com` a `www.contoso.com`, musí být schopný tooconfigure hello nastavení DNS pro hello `contoso.com` kořenové domény.

  > [!NOTE]
  > Pokud nemáte existující domény název, vezměte v úvahu [zakoupení domény pomocí portálu Azure hello](custom-dns-web-site-buydomains-web-app.md). 

## <a name="prepare-hello-app"></a>Příprava aplikace hello

toomap vlastní DNS název tooa webové aplikace, hello webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo  **Premium**). V tomto kroku budete zajistěte, aby tento hello v hello je aplikace služby App Service podporována cenovou úroveň.

### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure

Otevřete hello [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.

### <a name="navigate-toohello-app-in-hello-azure-portal"></a>Přejděte toohello aplikace v hello portálu Azure

Hello levé nabídce vyberte **App Services**a pak vyberte název hello aplikace hello.

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/select-app.png)

Zobrazí stránku hello správu hello aplikaci služby App Service.  

<a name="checkpricing"></a>

### <a name="check-hello-pricing-tier"></a>Zkontrolujte hello cenové úrovně

V levé navigační stránku hello aplikace hello, posuňte toohello **nastavení** a vyberte **škálování (plán služby App Service)**.

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

aktuální úroveň aplikace Hello je označený modré ohraničení. Zkontrolujte toomake se, že aplikace hello není hello **volné** vrstvy. Vlastní DNS není podporována v hello **volné** vrstvy. 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Pokud hello plán služby App Service není **volné**ukončit hello **zvolte cenovou úroveň** stránky a přeskočit příliš[mapování záznam CNAME](#cname).

<a name="scaleup"></a>

### <a name="scale-up-hello-app-service-plan"></a>Škálování hello plán služby App Service

Vyberte některé z vrstvy a bezplatnou hello (**sdílené**, **základní**, **standardní**, nebo **Premium**). 

Klikněte na **Vybrat**.

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Až uvidíte hello následující oznámení, je dokončena operace škálování hello.

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

<a name="cname"></a>

## <a name="map-a-cname-record"></a>Mapa záznam CNAME

V příkladu kurz hello přidat záznam CNAME pro hello `www` subdomény (například `www.contoso.com`).

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a>Vytvořit záznam CNAME hello

Přidat aplikaci subdomény toohello výchozí název hostitele toomap záznamů CNAME (`<app_name>.azurewebsites.net`).

Pro hello `www.contoso.com` příklad domény, přidejte záznam CNAME, který mapuje název hello `www` příliš`<app_name>.azurewebsites.net`.

Po přidání hello CNAME stránky záznamů DNS hello vypadá hello následující ukázka:

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/cname-record.png)

### <a name="enable-hello-cname-record-mapping-in-azure"></a>Povolit hello mapování záznam CNAME ve službě Azure

V hello levé navigační stránku hello aplikace v hello portálu Azure, vyberte **vlastní domény**. 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

V hello **vlastní domény** stránku hello aplikace a přidat hello plně kvalifikovaný název DNS vlastního (`www.contoso.com`) toohello seznamu.

Vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Typ hello plně kvalifikovaný název domény přidat záznam CNAME pro, jako například `www.contoso.com`. 

Vyberte **ověření**.

Hello **přidat název hostitele** se aktivuje tlačítko. 

Ujistěte se, že **typ záznamu Hostname** je nastaven příliš**CNAME (www.example.com nebo všechny subdomény)**.

Vyberte **přidat název hostitele**.

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky. Aktualizujte data hello tooupdate hello prohlížeče.

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v hello dolní části stránky hello.

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

<a name="a"></a>

## <a name="map-an-a-record"></a>Mapa záznamu a.

V příkladu hello kurzu přidáte záznamu A pro hello kořenové domény (například `contoso.com`). 

<a name="info"></a>

### <a name="copy-hello-apps-ip-address"></a>Zkopírujte aplikace hello IP adresu

záznam toomap A, musíte aplikace hello externí IP adresu. Tuto IP adresu můžete najít v aplikaci hello **vlastní domény** stránku hello portálu Azure.

V hello levé navigační stránku hello aplikace v hello portálu Azure, vyberte **vlastní domény**. 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

V hello **vlastní domény** stránky, zkopírujte aplikace hello IP adresu.

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/mapping-information.png)

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-a-record"></a>Vytvořit záznam A hello

toomap A záznamů tooan aplikace, služby App Service vyžaduje **dva** záznamy DNS:

- **A** záznam toomap toohello aplikace IP adresu.
- A **TXT** záznam hostname výchozí aplikace toohello toomap `<app_name>.azurewebsites.net`. Služby App Service používá tento záznam pouze v době konfigurace, tooverify, že vlastníte hello vlastní doménu. Jakmile vaši vlastní doménu ověřit a nakonfigurovat ve službě App Service, můžete odstranit tento záznam TXT. 

Pro hello `contoso.com` například domény vytvořit záznamy A a TXT hello podle následující tabulky toohello (`@` obvykle představuje hello kořenové domény). 

| Typ záznamu | Hostitel | Hodnota |
| - | - | - |
| A | `@` | IP adresu z [aplikace hello kopie IP adresu](#info) |
| TXT | `@` | `<app_name>.azurewebsites.net` |

Při přidávání záznamů hello, vypadá hello stránky záznamů DNS hello následující ukázka:

![Stránky záznamů DNS](./media/app-service-web-tutorial-custom-domain/a-record.png)

<a name="enable-a"></a>

### <a name="enable-hello-a-record-mapping-in-hello-app"></a>Povolit hello mapování záznam v aplikaci hello

Zpět v aplikaci hello **vlastní domény** stránku hello portálu Azure, přidejte hello plně kvalifikovaný název vlastního DNS (například `contoso.com`) toohello seznamu.

Vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name.png)

Typ hello plně kvalifikovaný název domény nakonfigurované hello A záznam, jako například `contoso.com`.

Vyberte **ověření**.

Hello **přidat název hostitele** se aktivuje tlačítko. 

Ujistěte se, že **typ záznamu Hostname** je nastaven příliš**záznam (example.com)**.

Vyberte **přidat název hostitele**.

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name.png)

Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky. Aktualizujte data hello tooupdate hello prohlížeče.

![Přidat záznam](./media/app-service-web-tutorial-custom-domain/a-record-added.png)

Pokud je vynechán krok nebo provedené překlepem někde dříve, se zobrazí chyba ověření v hello dolní části stránky hello.

![Chyba ověření](./media/app-service-web-tutorial-custom-domain/verification-error.png)

<a name="wildcard"></a>

## <a name="map-a-wildcard-domain"></a>Namapovat doménu zástupný znak

V příkladu kurz hello se mapují [název DNS zástupný znak](https://en.wikipedia.org/wiki/Wildcard_DNS_record) (například `*.contoso.com`) toohello aplikace App Service přidáním záznam CNAME. 

[!INCLUDE [Access DNS records with domain provider](../../includes/app-service-web-access-dns-records.md)]

### <a name="create-hello-cname-record"></a>Vytvořit záznam CNAME hello

Přidat aplikaci toohello na zástupný název výchozí název hostitele toomap záznamů CNAME (`<app_name>.azurewebsites.net`).

Pro hello `*.contoso.com` například domény hello záznam CNAME se mapování názvu hello `*` příliš`<app_name>.azurewebsites.net`.

Při přidání hello CNAME hello stránky záznamů DNS vypadá hello následující ukázka:

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/cname-record-wildcard.png)

### <a name="enable-hello-cname-record-mapping-in-hello-app"></a>Povolit hello mapování záznam CNAME v aplikaci hello

Nyní můžete přidat všechny subdomény, která odpovídá hello zástupný název toohello aplikace (například `sub1.contoso.com` a `sub2.contoso.com` odpovídat `*.contoso.com`). 

V hello levé navigační stránku hello aplikace v hello portálu Azure, vyberte **vlastní domény**. 

![Nabídky vlastní doménu.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

Vyberte hello  **+**  ikonu další příliš**přidat název hostitele**.

![Přidat název hostitele](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

Zadejte název plně kvalifikované domény, který odpovídá hello zástupné domény (například `sub1.contoso.com`) a potom vyberte **ověřením**.

Hello **přidat název hostitele** se aktivuje tlačítko. 

Ujistěte se, že **typ záznamu Hostname** je nastaven příliš**záznam CNAME (www.example.com nebo všechny subdomény)**.

Vyberte **přidat název hostitele**.

![Přidat aplikaci toohello název DNS](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname-wildcard.png)

Může trvat nějakou dobu hello nový název hostitele toobe promítnuta aplikace hello **vlastní domény** stránky. Aktualizujte data hello tooupdate hello prohlížeče.

Vyberte hello  **+**  ikonu znovu tooadd jiný název hostitele, odpovídající doméně hello zástupný znak. Například přidejte `sub2.contoso.com`.

![Přidat záznam CNAME](./media/app-service-web-tutorial-custom-domain/cname-record-added-wildcard2.png)

## <a name="test-in-browser"></a>Testování v prohlížeči

Procházet toohello názvy DNS, který jste nakonfigurovali dříve (například `contoso.com`, `www.contoso.com`, `sub1.contoso.com`, a `sub2.contoso.com`).

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/app-with-custom-dns.png)

## <a name="automate-with-scripts"></a>Automatizovat pomocí skriptů

Můžete automatizovat správu vlastních domén s skripty, pomocí hello [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview). 

### <a name="azure-cli"></a>Azure CLI 

Hello následující příkaz přidá nakonfigurované vlastní DNS název tooan aplikaci služby App Service. 

```bash 
az appservice web config hostname add \
    --webapp <app_name> \
    --resource-group <resource_group_name> \ 
    --name <fully_qualified_domain_name> 
``` 

Další informace najdete v tématu [namapovat vlastní doménu tooa webové aplikace](scripts/app-service-cli-configure-custom-domain.md). 

### <a name="azure-powershell"></a>Azure PowerShell 

Hello následující příkaz přidá nakonfigurované vlastní DNS název tooan aplikaci služby App Service. 

```PowerShell  
Set-AzureRmWebApp `
    -Name <app_name> `
    -ResourceGroupName <resource_group_name> ` 
    -HostNames @("<fully_qualified_domain_name>","<app_name>.azurewebsites.net") 
```

Další informace najdete v tématu [přiřadit vlastní domény tooa webové aplikace](scripts/app-service-powershell-configure-custom-domain.md).

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Mapa subdoména pomocí záznamu CNAME
> * Mapa kořenové domény pomocí záznamu a.
> * Mapa zástupné domény pomocí záznamu CNAME
> * Automatizovat mapování domény pomocí skriptů

Posunutí toohello další kurz toolearn jak toobind vlastní SSL certifikát tooa webové aplikace.

> [!div class="nextstepaction"]
> [Vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace](app-service-web-tutorial-custom-ssl.md)
