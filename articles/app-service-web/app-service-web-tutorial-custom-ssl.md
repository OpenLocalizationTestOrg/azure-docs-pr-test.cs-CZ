---
title: "Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps | Microsoft Docs"
description: "Naučte se vytvořit vazbu vlastní certifikát SSL pro webovou aplikaci, back-end mobilní aplikace nebo aplikace API v Azure App Service."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 5d5bf588-b0bb-4c6d-8840-1b609cfb5750
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/23/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 15c31ae5451a31dff2df08047ee43e75edacc127
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-to-azure-web-apps"></a>Vytvoření vazby stávající vlastní certifikát SSL pro službu Azure Web Apps

Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. V tomto kurzu se dozvíte, jak vytvořit vazbu vlastní certifikát SSL, který zakoupenému od důvěryhodné certifikační autority do [Azure Web Apps](app-service-web-overview.md). Jakmile budete hotovi, budete mít přístup k vaší webové aplikace na koncový bod HTTPS vaše vlastní doména DNS.

![Webové aplikace s vlastní certifikát SSL](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Upgrade cenová úroveň vaší aplikace
> * Vytvoření vazby svůj vlastní certifikát SSL do služby App Service
> * Vynutit HTTPS pro vaši aplikaci.
> * Automatizovat vazbu certifikátu SSL se skripty

> [!NOTE]
> Pokud potřebujete získat vlastní certifikát SSL, můžete získat na portálu Azure přímo a navázat jej do vaší webové aplikace. Postupujte podle [kurz služby App Service Certificate](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Požadavky

Pro absolvování tohoto kurzu potřebujete:

- [Vytvořit aplikaci služby App Service](/azure/app-service/)
- [Mapování názvu DNS na vaší webové aplikace](app-service-web-tutorial-custom-domain.md)
- Získat certifikát SSL od důvěryhodné certifikační autority

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Požadavky pro svůj certifikát SSL

Pro použití certifikátu ve službě App Service, certifikát musí splňovat následující požadavky:

* Podepsaný důvěryhodnou certifikační autoritou.
* Exportovat jako soubor PFX chráněný heslem
* Obsahuje privátní klíč minimálně 2048 bitů dlouho
* Obsahuje všechny zprostředkující certifikáty v řetězu certifikátů

> [!NOTE]
> **Elliptic Curve Cryptography (ECC) certifikáty** můžete pracovat s App Service, ale nejsou uvedené v tomto článku. Spolupracovat s vaší certifikační autority na přesné kroky k vytvoření certifikáty ECC.

## <a name="prepare-your-web-app"></a>Příprava vaší webové aplikace

K vytvoření vazby certifikát SSL a vlastní webové aplikace, vaše [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být v **základní**, **standardní**, nebo **Premium** vrstvy. V tomto kroku je třeba zkontrolovat, že vaše webová aplikace je v podporovaném cenová úroveň.

### <a name="log-in-to-azure"></a>Přihlaste se k Azure.

Otevřete web [Azure Portal](https://portal.azure.com).

### <a name="navigate-to-your-web-app"></a>Přejděte do vaší webové aplikace

V levé nabídce klikněte na tlačítko **App Services**a potom klikněte na název vaší webové aplikace.

![Výběr webové aplikace](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Dostali jste na stránce Správa webové aplikace.  

### <a name="check-the-pricing-tier"></a>Zkontrolujte cenové úrovně

V levém navigačním panelu webové stránky aplikace, přejděte na **nastavení** a vyberte **škálování (plán služby App Service)**.

![Škálování nabídky](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Zkontrolujte, že není součástí vaší webové aplikace **volné** nebo **sdílené** vrstvy. Aktuální úroveň vaší webové aplikace se zvýrazní v tmavý blue pole.

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Vlastní SSL není podporována v **volné** nebo **sdílené** vrstvy. Pokud potřebujete škálování, postupujte podle kroků v další části. Jinak, zavřete **zvolte cenovou úroveň** stránky a přejít na [odesílání a vaše certifikát SSL vazbu](#upload).

### <a name="scale-up-your-app-service-plan"></a>Škálovat plán služby App Service

Vyberte jednu z **základní**, **standardní**, nebo **Premium** vrstev.

Klikněte na **Vybrat**.

![Vyberte cenovou úroveň](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Když se zobrazí následující oznámení, je dokončena operace škálování.

![Škálování oznámení](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Vytvoření vazby certifikátu SSL

Jste připraveni k odeslání vašeho certifikátu SSL pro webovou aplikaci.

### <a name="merge-intermediate-certificates"></a>Sloučení zprostředkující certifikáty

Pokud certifikační autorita poskytuje několik certifikátů v řetězu certifikátů, musíte sloučit certifikáty v pořadí. 

Chcete-li to provést, otevřete každý certifikát, který jste obdrželi v textovém editoru. 

Vytvořte soubor pro sloučené certifikát názvem _mergedcertificate.crt_. V textovém editoru zkopírujte obsah každý certifikát do tohoto souboru. Pořadí certifikáty by měl vypadat podobně jako následující šablony:

```
-----BEGIN CERTIFICATE-----
<your Base64 encoded SSL certificate>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 1>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded intermediate certificate 2>
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
<Base64 encoded root certificate>
-----END CERTIFICATE-----
```

### <a name="export-certificate-to-pfx"></a>Export certifikátu PFX

Exportujte sloučené certifikát SSL s privátním klíčem, který žádost o certifikát byl vytvořen s.

Pokud jste vygenerovali žádosti o certifikát pomocí OpenSSL, jste vytvořili soubor privátního klíče. Chcete-li exportovat certifikát PFX, spusťte následující příkaz. Nahraďte zástupné symboly  _&lt;soubor privátního klíče >_ a  _&lt;sloučit soubor certifikátu >_.

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Po zobrazení výzvy zadejte heslo k exportu. Toto heslo budete používat při nahrávání svůj certifikát SSL do služby App Service později.

Pokud jste použili služby IIS nebo _Certreq.exe_ generovat žádosti o certifikát, nainstalujte certifikát do místního počítače a potom [exportu certifikátu do PFX](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Nahrajte certifikát SSL

Chcete-li nahrát svůj certifikát SSL, klikněte na tlačítko **certifikáty SSL** v levé navigaci vaší webové aplikace.

Klikněte na tlačítko **nahrát certifikát**.

V **soubor certifikátu PFX**, vyberte soubor PFX. V **heslo certifikátu**, zadejte heslo, které jste vytvořili při exportu souboru PFX.

Klikněte na **Odeslat**.

![Nahrání certifikátu](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v **certifikáty SSL** stránky.

![Nahrát certifikát](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Vytvoření vazby certifikátu SSL

V **vazby SSL** klikněte na tlačítko **přidat vazbu**.

V **přidat vazbu SSL** pomocí rozevíracích seznamů a vyberte název domény pro zabezpečení a certifikát, který chcete použít.

> [!NOTE]
> Pokud jste nahráli certifikát, ale nejsou zobrazeny názvy domény v **Hostname** rozevíracího seznamu, aktualizujte stránku prohlížeče.
>
>

V **SSL typ**, vyberte, zda chcete použít  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo SSL založenou na protokolu IP.

- **Založená na SNI** -vazby SSL na základě více SNI, mohou být přidány. Tato možnost umožňuje víc certifikátů SSL k zabezpečení více domén na stejnou IP adresu. Většina moderních prohlížeče (včetně aplikace Internet Explorer, Chrome, Firefox a Opera) podporují SNI (najít komplexnější informací podpory prohlížeče na [indikace názvu serveru](http://wikipedia.org/wiki/Server_Name_Indication)).
- **Založená na IP** -lze přidat pouze jednu vazbu SSL založenou na protokolu IP. Tato možnost umožňuje jenom jeden certifikát SSL k zabezpečení vyhrazené veřejné IP adresy. K zabezpečení více domén, je nutné zabezpečit je všechny používající stejný certifikát SSL. Toto je tradiční možnost pro vazbu SSL.

Klikněte na tlačítko **přidat vazbu**.

![Vytvoření vazby certifikátu SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v **vazby SSL** oddíly.

![Certifikát vázaný do webové aplikace](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Přemapování záznam pro protokol SSL IP

Pokud nepoužíváte založená na IP ve vaší webové aplikaci, přejděte k [HTTPS Test pro vaše vlastní doména](#test).

Ve výchozím nastavení používá sdílené veřejnou IP adresu webové aplikace. Při vytvoření vazby certifikátu SSL založenou na protokolu IP, služby App Service vytvoří nový, vyhrazenou IP adresu pro vaši webovou aplikaci.

Pokud záznam jsou namapovány na vaší webové aplikace, aktualizace registru vaší domény pomocí této nové, vyhrazené IP adresy.

Webové aplikace **vlastní domény** stránka se aktualizuje pomocí nové, vyhrazené IP adresy. [Zkopírujte tuto IP adresu](app-service-web-tutorial-custom-domain.md#info), pak [přemapovat záznam A](app-service-web-tutorial-custom-domain.md#map-an-a-record) na tuto novou IP adresu.

<a name="test"></a>

## <a name="test-https"></a>Test HTTPS

Nyní již zbývá udělat je a ujistěte se, že funguje protokol HTTPS pro vaši vlastní doménu. V různých prohlížečích, přejděte do `https://<your.custom.domain>` zobrazíte vykonává až vaše webová aplikace.

![Portál navigace do aplikace Azure](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Pokud vaše webová aplikace obsahuje certifikát chyby ověření, pravděpodobně používáte certifikát podepsaný svým držitelem.
>
> Pokud není tento případ, může mít vynecháno zprostředkující certifikáty při exportu certifikátu do souboru PFX.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>Vynucení HTTPS

Služba aplikace nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP. Pokud chcete vynutit HTTPS pro vaši webovou aplikaci, definovat pravidla přepsání v _web.config_ souboru pro vaši webovou aplikaci. Služby App Service používá tento soubor, bez ohledu na jazykové rozhraní vaší webové aplikace.

> [!NOTE]
> Neexistuje konkrétní jazyk přesměrování požadavků. Můžete použít ASP.NET MVC [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtru místo přepsání pravidla v _web.config_.

Pokud jste vývojář .NET, byste měli být relativně obeznámeni s tohoto souboru. Je v kořenu vašeho řešení.

Případně pokud vyvíjíte pomocí PHP, Node.js, Python nebo Java, existuje pravděpodobnost, tento soubor vaším jménem jsme generované ve službě App Service.

Připojení ke koncovému bodu webové aplikace FTP podle pokynů v [nasazení vaší aplikace do Azure App Service pomocí FTP nebo S](app-service-deploy-ftp.md).

Tento soubor se musí nacházet ve _/home/site/wwwroot_. Pokud ne, vytvořte _web.config_ soubor v této složce se souborem XML následující:

```xml   
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
      <rules>
        <!-- BEGIN rule ELEMENT FOR HTTPS REDIRECT -->
        <rule name="Force HTTPS" enabled="true">
          <match url="(.*)" ignoreCase="false" />
          <conditions>
            <add input="{HTTPS}" pattern="off" />
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent" />
        </rule>
        <!-- END rule ELEMENT FOR HTTPS REDIRECT -->
      </rules>
    </rewrite>
  </system.webServer>
</configuration>
```

Pro existující _web.config_ souboru, zkopírujte celou `<rule>` element do vaší _web.config_na `configuration/system.webServer/rewrite/rules` elementu. Pokud existují další `<rule>` elementů ve vaší _web.config_, umístit zkopírovaný `<rule>` element před dalších `<rule>` elementy.

Toto pravidlo vrátí HTTP 301 (trvalé přesměrování) pro protokol HTTPS, vždy, když uživatel provede požadavek HTTP do vaší webové aplikace. Například přesměruje z `http://contoso.com` k `https://contoso.com`.

Další informace o modul IIS URL Rewrite najdete v tématu [přepisování adres URL](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentaci.

## <a name="enforce-https-for-web-apps-on-linux"></a>Vynutit HTTPS pro webové aplikace v systému Linux

Aplikační služby v systému Linux nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP. Pokud chcete vynutit HTTPS pro vaši webovou aplikaci, definovat pravidla přepsání v _.htaccess z_ souboru pro vaši webovou aplikaci. 

Připojení ke koncovému bodu webové aplikace FTP podle pokynů v [nasazení vaší aplikace do Azure App Service pomocí FTP nebo S](app-service-deploy-ftp.md).

V _/home/site/wwwroot_, vytvořit _.htaccess z_ soubor s následujícím kódem:

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

Toto pravidlo vrátí HTTP 301 (trvalé přesměrování) pro protokol HTTPS, vždy, když uživatel provede požadavek HTTP do vaší webové aplikace. Například přesměruje z `http://contoso.com` k `https://contoso.com`.

## <a name="automate-with-scripts"></a>Automatizovat pomocí skriptů

Je možné automatizovat vazby SSL pro webovou aplikaci pomocí skriptů, pomocí [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

Následující příkaz odešle exportovaný soubor PFX a získá kryptografický otisk.

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

Následující příkaz přidá vazby SSL na základě SNI, pomocí kryptografického otisku z předchozí příkaz.

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

Následující příkaz odešle exportovaný soubor PFX a přidá vazbu na základě SNI SSL.

```PowerShell
New-AzureRmWebAppSSLBinding `
    -WebAppName <app_name> `
    -ResourceGroupName <resource_group_name> `
    -Name <dns_name> `
    -CertificateFilePath <path_to_PFX_file> `
    -CertificatePassword <PFX_password> `
    -SslState SniEnabled
```

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili:

> [!div class="checklist"]
> * Upgrade cenová úroveň vaší aplikace
> * Vytvoření vazby svůj vlastní certifikát SSL do služby App Service
> * Vynutit HTTPS pro vaši aplikaci.
> * Automatizovat vazbu certifikátu SSL se skripty

Přechodu na v dalším kurzu se dozvíte, jak používat Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Přidat síti pro doručování obsahu (CDN) do Azure App Service](app-service-web-tutorial-content-delivery-network.md)
