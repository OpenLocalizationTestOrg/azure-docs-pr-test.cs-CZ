---
title: "aaaBind existující vlastní SSL certifikát tooAzure Web Apps | Microsoft Docs"
description: "Přečtěte si tootoobind vlastní SSL certifikát tooyour webové aplikace, back-end mobilní aplikace nebo aplikace API v Azure App Service."
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
ms.openlocfilehash: 3503ba9f96c8ea8d18451e8bf9a9b441797ef44d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="bind-an-existing-custom-ssl-certificate-tooazure-web-apps"></a>Vytvořit vazbu existující vlastní SSL certifikát tooAzure webové aplikace

Azure Web Apps nabízí vysoce škálovatelnou a automatických oprav webové hostitelské služby. Tento kurz ukazuje, jak toobind vlastní SSL certifikát, který jste zakoupenému od důvěryhodné certifikační autority příliš[Azure Web Apps](app-service-web-overview.md). Jakmile budete hotovi, budete moct tooaccess vaší webové aplikace na koncový bod HTTPS hello vlastní domény DNS.

![Webové aplikace s vlastní certifikát SSL](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Upgrade cenová úroveň vaší aplikace
> * Vytvoření vazby vaše vlastní tooApp certifikát SSL služby
> * Vynutit HTTPS pro vaši aplikaci.
> * Automatizovat vazbu certifikátu SSL se skripty

> [!NOTE]
> Pokud potřebujete tooget vlastní certifikát SSL, můžete získat v hello portál Azure přímo a navázat jej tooyour webové aplikace. Postupujte podle hello [kurz služby App Service Certificate](web-sites-purchase-ssl-web-site.md).

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

- [Vytvořit aplikaci služby App Service](/azure/app-service/)
- [Mapa vlastní DNS název tooyour webové aplikace](app-service-web-tutorial-custom-domain.md)
- Získat certifikát SSL od důvěryhodné certifikační autority

<a name="requirements"></a>

### <a name="requirements-for-your-ssl-certificate"></a>Požadavky pro svůj certifikát SSL

toouse certifikát ve službě App Service, hello certifikátu musí splňovat všechny hello následující požadavky:

* Podepsaný důvěryhodnou certifikační autoritou.
* Exportovat jako soubor PFX chráněný heslem
* Obsahuje privátní klíč minimálně 2048 bitů dlouho
* Obsahuje všechny zprostředkující certifikáty v řetězu certifikátů hello

> [!NOTE]
> **Elliptic Curve Cryptography (ECC) certifikáty** můžete pracovat s App Service, ale nejsou uvedené v tomto článku. Spolupracovat s vaší certifikační autority na certifikáty ECC toocreate přesný postup hello.

## <a name="prepare-your-web-app"></a>Příprava vaší webové aplikace

toobind vlastní SSL certifikát tooyour webové aplikace, vaše [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být v hello **základní**, **standardní**, nebo **Premium** vrstvy. V tomto kroku budete zajistěte, aby že vaší webové aplikace v hello podporován cenová úroveň.

### <a name="log-in-tooazure"></a>Přihlaste se tooAzure

Otevřete hello [portál Azure](https://portal.azure.com).

### <a name="navigate-tooyour-web-app"></a>Přejděte tooyour webové aplikace

V levé nabídce hello, klikněte na **App Services**a potom klikněte na název hello vaší webové aplikace.

![Výběr webové aplikace](./media/app-service-web-tutorial-custom-ssl/select-app.png)

Dostali jste na stránce Správa hello vaší webové aplikace.  

### <a name="check-hello-pricing-tier"></a>Zkontrolujte hello cenové úrovně

V levém navigačním hello webové stránky aplikace, posuňte toohello **nastavení** a vyberte **škálování (plán služby App Service)**.

![Škálování nabídky](./media/app-service-web-tutorial-custom-ssl/scale-up-menu.png)

Zkontrolujte toomake se, že vaše webová aplikace není hello **volné** nebo **sdílené** vrstvy. Aktuální úroveň vaší webové aplikace se zvýrazní v tmavý blue pole.

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-ssl/check-pricing-tier.png)

Vlastní SSL není podporována v hello **volné** nebo **sdílené** vrstvy. Pokud potřebujete tooscale nahoru, postupujte podle kroků hello v další části hello. Jinak, zavřete hello **zvolte cenovou úroveň** stránky a přeskočit příliš[odesílání a vaše certifikát SSL vazbu](#upload).

### <a name="scale-up-your-app-service-plan"></a>Škálovat plán služby App Service

Vyberte jednu z hello **základní**, **standardní**, nebo **Premium** vrstev.

Klikněte na **Vybrat**.

![Vyberte cenovou úroveň](./media/app-service-web-tutorial-custom-ssl/choose-pricing-tier.png)

Až uvidíte hello následující oznámení, je dokončena operace škálování hello.

![Škálování oznámení](./media/app-service-web-tutorial-custom-ssl/scale-notification.png)

<a name="upload"></a>

## <a name="bind-your-ssl-certificate"></a>Vytvoření vazby certifikátu SSL

Můžete jsou připravené tooupload vaší webové aplikace tooyour certifikát SSL.

### <a name="merge-intermediate-certificates"></a>Sloučení zprostředkující certifikáty

Pokud certifikační autorita poskytuje několik certifikátů v řetězu certifikátů hello, je třeba toomerge hello certifikáty v pořadí. 

toodo to otevřete každý certifikát vám zobrazily v textovém editoru. 

Vytvořte soubor pro hello sloučené certifikát nazvaný _mergedcertificate.crt_. V textovém editoru zkopírujte obsah hello jednotlivých certifikátů do tohoto souboru. pořadí Hello certifikáty by měl vypadat podobně jako hello následující šablony:

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

### <a name="export-certificate-toopfx"></a>Export certifikátu tooPFX

Exportujte sloučené certifikát SSL s hello privátní klíč, který žádost o certifikát se vygeneroval s.

Pokud jste vygenerovali žádosti o certifikát pomocí OpenSSL, jste vytvořili soubor privátního klíče. tooexport váš certifikát tooPFX, spusťte následující příkaz hello. Nahraďte zástupné symboly hello  _&lt;soubor privátního klíče >_ a  _&lt;sloučit soubor certifikátu >_.

```
openssl pkcs12 -export -out myserver.pfx -inkey <private-key-file> -in <merged-certificate-file>  
```

Po zobrazení výzvy zadejte heslo k exportu. Toto heslo budete používat při nahrávání vaší SSL certifikát tooApp služby později.

Pokud jste použili služby IIS nebo _Certreq.exe_ toogenerate žádost o certifikát, nainstalujte hello certifikát tooyour místního počítače a potom [exportovat certifikát tooPFX hello](https://technet.microsoft.com/library/cc754329(v=ws.11).aspx).

### <a name="upload-your-ssl-certificate"></a>Nahrajte certifikát SSL

tooupload svůj certifikát SSL, klikněte na tlačítko **certifikáty SSL** v hello levé navigační vaší webové aplikace.

Klikněte na tlačítko **nahrát certifikát**.

V **soubor certifikátu PFX**, vyberte soubor PFX. V **heslo certifikátu**, zadejte heslo hello, kterou jste vytvořili při exportu souboru PFX hello.

Klikněte na **Odeslat**.

![Nahrání certifikátu](./media/app-service-web-tutorial-custom-ssl/upload-certificate.png)

Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v hello **certifikáty SSL** stránky.

![Nahrát certifikát](./media/app-service-web-tutorial-custom-ssl/certificate-uploaded.png)

### <a name="bind-your-ssl-certificate"></a>Vytvoření vazby certifikátu SSL

V hello **vazby SSL** klikněte na tlačítko **přidat vazbu**.

V hello **přidat vazbu SSL** stránky, použijte hello rozevíracích seznamů tooselect hello domény název toosecure a toouse certifikát hello.

> [!NOTE]
> Pokud jste nahráli certifikát, ale nejsou zobrazeny názvy domény hello v hello **Hostname** rozevírací seznam, zkuste aktualizovat stránku hello prohlížeče.
>
>

V **SSL typ**, vyberte zda toouse  **[indikace názvu serveru (SNI)](http://en.wikipedia.org/wiki/Server_Name_Indication)**  nebo SSL založenou na protokolu IP.

- **Založená na SNI** -vazby SSL na základě více SNI, mohou být přidány. Tato možnost umožňuje více toosecure certifikáty SSL více domén na hello stejnou IP adresu. Většina moderních prohlížeče (včetně aplikace Internet Explorer, Chrome, Firefox a Opera) podporují SNI (najít komplexnější informací podpory prohlížeče na [indikace názvu serveru](http://wikipedia.org/wiki/Server_Name_Indication)).
- **Založená na IP** -lze přidat pouze jednu vazbu SSL založenou na protokolu IP. Tato možnost umožňuje jenom jednu toosecure certifikát SSL vyhrazený veřejnou IP adresu. toosecure několik domén, musíte zabezpečit je všechny pomocí hello stejný certifikát protokolu SSL. Toto je tradiční možnost hello pro vazbu SSL.

Klikněte na tlačítko **přidat vazbu**.

![Vytvoření vazby certifikátu SSL](./media/app-service-web-tutorial-custom-ssl/bind-certificate.png)

Po dokončení nahrávání svůj certifikát služby App Service se zobrazí v hello **vazby SSL** oddíly.

![Certifikát vázaný tooweb aplikace](./media/app-service-web-tutorial-custom-ssl/certificate-bound.png)

## <a name="remap-a-record-for-ip-ssl"></a>Přemapování záznam pro protokol SSL IP

Pokud nepoužíváte založená na IP ve vaší webové aplikaci, přeskočte příliš[HTTPS Test pro vaše vlastní doména](#test).

Ve výchozím nastavení používá sdílené veřejnou IP adresu webové aplikace. Při vytvoření vazby certifikátu SSL založenou na protokolu IP, služby App Service vytvoří nový, vyhrazenou IP adresu pro vaši webovou aplikaci.

Pokud jsou namapovány webové aplikace A záznamů tooyour, aktualizujte registr domény pomocí této nové, vyhrazené IP adresy.

Webové aplikace **vlastní domény** stránka je aktualizována hello nové, vyhrazenou IP adresu. [Zkopírujte tuto IP adresu](app-service-web-tutorial-custom-domain.md#info), pak [přemapování hello záznam](app-service-web-tutorial-custom-domain.md#map-an-a-record) toothis novou IP adresu.

<a name="test"></a>

## <a name="test-https"></a>Test HTTPS

Všechno, co teď opustil toodo je toomake se, že pracuje HTTPS pro vaši vlastní doménu. V různých prohlížečích, procházet příliš`https://<your.custom.domain>` toosee, který ji obsluhuje až vaše webová aplikace.

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-ssl/app-with-custom-ssl.png)

> [!NOTE]
> Pokud vaše webová aplikace obsahuje certifikát chyby ověření, pravděpodobně používáte certifikát podepsaný svým držitelem.
>
> Pokud to není hello případ, může mít vynecháno zprostředkující certifikáty při exportu souboru PFX toohello vašeho certifikátu.

<a name="bkmk_enforce"></a>

## <a name="enforce-https"></a>Vynucení HTTPS

Služba aplikace nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP. definování tooenforce HTTPS pro webovou aplikaci, přepište pravidlo v hello _web.config_ souboru pro vaši webovou aplikaci. Služby App Service používá tento soubor, bez ohledu na jazykové rozhraní hello vaší webové aplikace.

> [!NOTE]
> Neexistuje konkrétní jazyk přesměrování požadavků. ASP.NET MVC můžete použít hello [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtru místo hello přepište pravidlo v _web.config_.

Pokud jste vývojář .NET, byste měli být relativně obeznámeni s tohoto souboru. Je v kořenovém hello vašeho řešení.

Případně pokud vyvíjíte pomocí PHP, Node.js, Python nebo Java, existuje pravděpodobnost, tento soubor vaším jménem jsme generované ve službě App Service.

Koncový bod FTP tooyour webovou aplikaci připojit podle následujících pokynů hello [nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S](app-service-deploy-ftp.md).

Tento soubor se musí nacházet ve _/home/site/wwwroot_. Pokud ne, vytvořte _web.config_ soubor v této složce s hello následující XML:

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

Pro existující _web.config_ souboru, zkopírujte celou hello `<rule>` element do vaší _web.config_na `configuration/system.webServer/rewrite/rules` elementu. Pokud existují další `<rule>` elementů ve vaší _web.config_, místní hello zkopírovat `<rule>` element před hello jiných `<rule>` elementy.

Toto pravidlo vrátí protokol HTTP 301 (trvalé přesměrování) toohello HTTPS, vždy, když uživatel hello provede webové aplikace HTTP požadavku tooyour. Například přesměruje z `http://contoso.com` příliš`https://contoso.com`.

Další informace o modul IIS URL Rewrite hello najdete v tématu hello [přepisování adres URL](http://www.iis.net/downloads/microsoft/url-rewrite) dokumentaci.

## <a name="enforce-https-for-web-apps-on-linux"></a>Vynutit HTTPS pro webové aplikace v systému Linux

Aplikační služby v systému Linux nemá *není* vynutit protokolu HTTPS, takže všichni uživatelé stále přístup k vaší webové aplikace pomocí protokolu HTTP. definování tooenforce HTTPS pro webovou aplikaci, přepište pravidlo v hello _.htaccess z_ souboru pro vaši webovou aplikaci. 

Koncový bod FTP tooyour webovou aplikaci připojit podle následujících pokynů hello [nasazení vaší aplikace tooAzure služby App Service pomocí FTP nebo S](app-service-deploy-ftp.md).

V _/home/site/wwwroot_, vytvořit _.htaccess z_ soubor s hello následující kód:

```
RewriteEngine On
RewriteCond %{HTTP:X-ARR-SSL} ^$
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

Toto pravidlo vrátí protokol HTTP 301 (trvalé přesměrování) toohello HTTPS, vždy, když uživatel hello provede webové aplikace HTTP požadavku tooyour. Například přesměruje z `http://contoso.com` příliš`https://contoso.com`.

## <a name="automate-with-scripts"></a>Automatizovat pomocí skriptů

Je možné automatizovat vazby SSL pro webovou aplikaci pomocí skriptů, pomocí hello [rozhraní příkazového řádku Azure](/cli/azure/install-azure-cli) nebo [prostředí Azure PowerShell](/powershell/azure/overview).

### <a name="azure-cli"></a>Azure CLI

Následující příkaz Hello odešle exportovaný soubor PFX a získá hello kryptografický otisk.

```bash
thumbprint=$(az appservice web config ssl upload \
    --name <app_name> \
    --resource-group <resource_group_name> \
    --certificate-file <path_to_PFX_file> \
    --certificate-password <PFX_password> \
    --query thumbprint \
    --output tsv)
```

Hello následující příkaz přidá vazby SSL na základě SNI, pomocí kryptografického otisku hello z předchozí příkaz hello.

```bash
az appservice web config ssl bind \
    --name <app_name> \
    --resource-group <resource_group_name>
    --certificate-thumbprint $thumbprint \
    --ssl-type SNI \
```

### <a name="azure-powershell"></a>Azure PowerShell

Hello následující příkaz odešle exportovaný soubor PFX a přidá vazbu na základě SNI SSL.

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
> * Vytvoření vazby vaše vlastní tooApp certifikát SSL služby
> * Vynutit HTTPS pro vaši aplikaci.
> * Automatizovat vazbu certifikátu SSL se skripty

Jak zálohy další kurz toolearn toohello toouse Azure Content Delivery Network.

> [!div class="nextstepaction"]
> [Přidat tooan Content Delivery Network (CDN) Azure App Service](app-service-web-tutorial-content-delivery-network.md)
