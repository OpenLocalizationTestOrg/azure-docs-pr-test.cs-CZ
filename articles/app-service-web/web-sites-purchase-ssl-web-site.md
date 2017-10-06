---
title: "aaaAdd protokolem SSL certifikát tooyour aplikace Azure App Service | Microsoft Docs"
description: "Zjistěte, jak tooadd protokolem SSL certifikát tooyour aplikaci služby App Service."
services: app-service
documentationcenter: .net
author: ahmedelnably
manager: stefsch
editor: cephalin
tags: buy-ssl-certificates
ms.assetid: cdb9719a-c8eb-47e5-817f-e15eaea1f5f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2016
ms.author: apurvajo
ms.openlocfilehash: f4652794ba745790a073264f6a102c64c73e8db0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="buy-and-configure-an-ssl-certificate-for-your-azure-app-service"></a>Koupě a konfigurace certifikátu SSL pro službu Azure App Service

V tomto kurzu bude Zabezpečte svoji webovou aplikaci pomocí nákupu certifikát protokolu SSL pro vaše  **[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)**, bezpečně uložením do [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis)a přiřadí se s vlastní doménu.

## <a name="step-1---log-in-tooazure"></a>Krok 1 – přihlášení tooAzure

Přihlaste se toohello portál Azure na http://portal.azure.com

## <a name="step-2---place-an-ssl-certificate-order"></a>Krok 2 – umístit pořadí certifikát SSL

Certifikát SSL pořadí můžete umístit tak, že vytvoříte novou [certifikát služby aplikace](https://portal.azure.com/#create/Microsoft.SSL) v hello **portál Azure**.

![Vytvoření certifikátu](./media/app-service-web-purchase-ssl-web-site/createssl.png)

Zadejte popisný **název** certifikátů pro zabezpečení SSL a zadejte hello **název domény**

> [!NOTE]
> To je jedním z nejdůležitějších části hello procesu nákupu hello. Ujistěte se, že tooenter Opravte název hostitele (vlastní domény), které chcete tooprotect s tímto certifikátem. **NECHCETE** připojit hello název hostitele s WWW. 
>

Vyberte vaše **předplatné**, **skupiny prostředků**, a **certifikátu SKU**

> [!WARNING]
> Aplikace služby certifikáty lze použít pouze v případě ostatních služeb aplikace v rámci hello stejného předplatného.  
>

## <a name="step-3---store-hello-certificate-in-azure-key-vault"></a>Krok 3 – certifikát hello úložiště v Azure Key Vault

> [!NOTE]
> [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) je služba Azure, která pomáhá chránit kryptografické klíče a tajné klíče používané cloudovými aplikacemi a službami.
>

Po dokončení nákupu certifikát SSL hello potřebujete tooopen [služby App Service Certificate](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.CertificateRegistration%2FcertificateOrders) okna prostředků.

![Vložit obrázek připravené toostore v KV](./media/app-service-web-purchase-ssl-web-site/ReadyKV.png)

Si všimnete, že je stav certifikátu **"Čekající vystavení"** jak několika více kroků toocomplete musíte před zahájením s použitím tohoto certifikátu.

Klikněte na tlačítko **konfigurace certifikátu** v okně Vlastnosti certifikátu a klikněte na **krok 1: úložiště** toostore tento certifikát v Azure Key Vault.

Z **klíč trezoru stav** okně klikněte na tlačítko **klíč trezoru úložiště** toochoose existující toostore Key Vault tento certifikát **nebo vytvořit nový klíč trezoru** toocreate nový klíč trezoru ve stejném předplatném nebo skupině prostředků.

> [!NOTE]
> Azure Key Vault má minimální poplatky za uložení tohoto certifikátu.
> Další informace najdete v tématu  **[klíč trezoru podrobnostmi o cenách Azure](https://azure.microsoft.com/pricing/details/key-vault/)**.
>

Jakmile vyberete hello klíč trezoru úložiště toostore tento certifikát, hello **úložiště** možnost by se měla zobrazit úspěch.

![Vložit obrázek úspěchu úložiště v KV](./media/app-service-web-purchase-ssl-web-site/KVStoreSuccess.png)

## <a name="step-4---verify-hello-domain-ownership"></a>Krok 4 – ověření hello vlastnictví domény

> [!NOTE]
> Existují tři typy ověření domény nepodporuje App service Certificate: ověření domény, e-mailu, ručně. Tyto mechanismy jsou popsány v další podrobnosti v hello [Advanced části](#advanced).

Z hello stejné **konfigurace certifikátu** okno, které jste použili v kroku 3, klikněte na tlačítko **krok 2: ověření**.

**Ověření domény** je nejvhodnější proces hello **pouze v případě** máte  **[zakoupili vaši vlastní doménu služby Azure App Service.](custom-dns-web-site-buydomains-web-app.md)**
Klikněte na **ověřte** tlačítko toocomplete tento krok.

![Vložit obrázek ověření domény](./media/app-service-web-purchase-ssl-web-site/DomainVerificationRequired.png)

Po kliknutí na **ověřte**, použijte hello **aktualizovat** tlačítko dokud hello **ověřte** možnost by se měla zobrazit úspěch.

![Vložit obrázek ověřit úspěšné provedení v KV](./media/app-service-web-purchase-ssl-web-site/KVVerifySuccess.png)

## <a name="step-5---assign-certificate-tooapp-service-app"></a>Krok 5 – přiřadit certifikát tooApp služby App Service

> [!NOTE]
> Před provedením kroků hello v této části, musí mít přidružené vlastního názvu domény s vaší aplikací. Další informace najdete v tématu  **[konfigurace vlastního názvu domény pro webovou aplikaci.](app-service-web-tutorial-custom-domain.md)**
>

V hello  **[portál Azure](https://portal.azure.com/)**, klikněte na tlačítko hello **služby App Service** možnost na levé straně hello stránku hello.

Klikněte na název hello toowhich vaší aplikace, kterou chcete tooassign tento certifikát.

V hello **nastavení**, klikněte na tlačítko **certifikáty SSL**.

Klikněte na tlačítko **importovat aplikaci služby certifikát** a vyberte hello certifikátů, které jste zakoupili.

![Vložit obrázek importovat certifikát](./media/app-service-web-purchase-ssl-web-site/ImportCertificate.png)

V hello **vazby ssl** část kliknutím na **přidat vazby**, použijte hello rozevíracích seznamů tooselect hello domény název toosecure pomocí protokolu SSL a hello toouse certifikátu. Můžete také vybrat zda toouse  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo IP na základě protokolu SSL.

![Vložit obrázek vazby SSL](./media/app-service-web-purchase-ssl-web-site/SSLBindings.png)

Klikněte na tlačítko **přidat vazbu** toosave hello změny a povolit protokol SSL.

> [!NOTE]
> Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, musíte provést následující další kroky hello. Tyto mechanismy jsou popsány v další podrobnosti v hello [Advanced části](#Advanced).

V tomto okamžiku jste by měl být schopný toovisit vaší aplikace pomocí `HTTPS://` místo `HTTP://` tooverify, který hello certifikát nebyl nakonfigurován správně.

<!--![insert image of https](./media/app-service-web-purchase-ssl-web-site/Https.png)-->

## <a name="step-6---management-tasks"></a>Krok 6: úlohy správy

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom SSL certificate to a web app")]

## <a name="advanced"></a>Advanced

### <a name="verifying-domain-ownership"></a>Ověření vlastnictví domény

Existují dva další typy ověření domény nepodporuje App service Certificate: e-mailu a ruční ověření.

#### <a name="mail-verification"></a>Ověření e-mailu

Ověření e-mailu už jsou poslané toohello e-mailové adresy přidruženého tuto vlastní doménu.
krok ověření e-mailu toocomplete hello, otevřete hello e-mailu a klikněte na odkaz ověření hello.

![Vložit obrázek ověření e-mailu](./media/app-service-web-purchase-ssl-web-site/KVVerifyEmailSuccess.png)

Pokud potřebujete tooresend hello ověření e-mailu, klikněte na tlačítko hello **znovu odeslat E-mail** tlačítko.

#### <a name="manual-verification"></a>Ruční ověření

> [!IMPORTANT]
> Ověření webové stránky HTML (funguje pouze u standardní SKU certifikát)
>

1. Vytvořte soubor HTML s názvem **"starfield.html"**

1. Obsah tohoto souboru musí být hello přesný název hello domény ověření tokenu. (Hello token můžete zkopírovat z hello okno Stav ověření domény)

1. Odeslat tento soubor v kořenovém hello hello webového serveru, který je hostitelem vaší domény`/.well-known/pki-validation/starfield.html`

1. Klikněte na tlačítko **aktualizovat** tooupdate hello certifikát stav po dokončení ověření. Může trvat několik minut toocomplete ověření.

> [!TIP]
> Ověřte v terminálu pomocí `curl -G http://<domain>/.well-known/pki-validation/starfield.html` hello odpověď by měla obsahovat hello `<verification-token>`.

#### <a name="dns-txt-record-verification"></a>Záznam ověření DNS TXT

1. Pomocí Správce DNS vytvořit záznam TXT na hello `@` subdomény s hodnota rovna toohello domény ověření tokenu.
1. Klikněte na tlačítko **"Aktualizovat"** tooupdate hello stav certifikátu po dokončení ověření.

> [!TIP]
> Musíte toocreate záznam TXT na `@.<domain>` s hodnotou `<verification-token>`.

### <a name="assign-certificate-tooapp-service-app"></a>Přiřadit certifikát tooApp služby App Service

Pokud jste vybrali **IP na základě SSL** a vaše vlastní doména je nakonfigurována pomocí záznamu A, musíte provést následující další kroky hello:

Po dokončení konfigurace IP adresy na základě vazby SSL, vyhrazenou IP adresu nebo je přiřazen tooyour aplikace. Tuto IP adresu můžete najít na hello **vlastní domény** stránky v části Nastavení aplikace, vpravo nahoře hello **názvy hostitelů** části. Je uveden jako **externí IP adresu**

![Vložit obrázek IP protokolu SSL](./media/app-service-web-purchase-ssl-web-site/virtual-ip-address.png)

Všimněte si, že tato IP adresa je jiné než hello virtuální IP adresy použít dříve tooconfigure hello A záznam pro vaši doménu. Pokud jste nakonfigurovali toouse SNI na základě protokolu SSL, nebo nejsou nakonfigurovány toouse SSL, je uvedena žádnou adresu pro tuto položku.

Pomocí nástrojů hello vaším registrátorem názvu domény, upravte hello záznam pro vaše vlastní doména název toopoint toohello IP adresu z předchozího kroku hello.

## <a name="rekey-and-sync-hello-certificate"></a>Změna hodnoty klíče a synchronizovat hello certifikátu

Pokud byste někdy potřebovali tooRekey svůj certifikát, vyberte **změna hodnoty klíče a synchronizovat** možnost z **vlastnosti certifikátu** okno.

Klikněte na tlačítko **změna hodnoty klíče** tlačítko tooinitiate hello procesu. Tento proces může trvat 1 až 10 minut toocomplete.

![Vložit obrázek změna hodnoty klíče protokolu SSL](./media/app-service-web-purchase-ssl-web-site/Rekey.png)

Obnovení klíčů vašeho certifikátu vrátí hello certifikát s nový certifikát vydaný certifikační autority hello.

## <a name="next-steps"></a>Další kroky

* [Přidat síti pro doručování obsahu](app-service-web-tutorial-content-delivery-network.md)