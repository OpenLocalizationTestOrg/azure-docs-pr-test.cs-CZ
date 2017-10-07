---
title: "aaaIntegrate Azure DNS s vašich prostředků Azure | Microsoft Docs"
description: "Zjistěte, jak toouse Azure DNS podél tooprovide DNS pro vašich prostředků Azure."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: gwallace
ms.openlocfilehash: b9b6f829513f0ad9da510190c75bc60dc7431545
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-dns-tooprovide-custom-domain-settings-for-an-azure-service"></a>Použít nastavení vlastní domény tooprovide Azure DNS pro službu Azure

Azure DNS poskytuje DNS pro vlastní doménu pro některé z vašich prostředků Azure, podporu vlastní domény nebo který mít platný plně kvalifikovaný název domény (FQDN). Příkladem je máte webové aplikace Azure a chcete, aby vaši uživatelé tooaccess ho a to buď pomocí contoso.com nebo www.contoso.com jako plně kvalifikovaný název domény. Tento článek vás provede procesem konfigurace služby Azure s Azure DNS pro používání vlastní domény.

## <a name="prerequisites"></a>Požadavky

V pořadí toouse Azure DNS pro vaši vlastní doménu musíte nejprve delegovat tooAzure vaší domény DNS. Navštivte [delegovat tooAzure domény DNS](./dns-delegate-domain-azure-dns.md) pokyny tooconfigure vaše názvové servery pro delegování. Jakmile doménu delegované tooyour zóny DNS, jste záznamy DNS schopný tooconfigure hello potřeby.

Můžete nakonfigurovat jednoduché nebo vlastní doména pro [Azure funkce aplikace](#azure-function-app), [Azure IoT](#azure-iot), [veřejné IP adresy](#public-ip-address), [služby App Service (webové aplikace)](#app-service-web-apps), [Úložiště objektů blob](#blob-storage), a [Azure CDN](#azure-cdn).

## <a name="azure-function-app"></a>Aplikace Azure – funkce

tooconfigure vlastní doménu pro Azure funkce aplikace, a také konfigurace na hello funkce aplikace se vytvoří záznam CNAME.
 
Přejděte příliš**jiných** > **aplikaci funkce** a vyberte svou aplikaci funkce. Klikněte na tlačítko **funkce** a v části **sítě** klikněte na tlačítko **vlastní domény**.

![okno aplikace – funkce](./media/dns-custom-domain/functionapp.png)

Poznámka: adresy url aktuální hello na hello **vlastní domény** okně tato adresa se používá jako hello alias vytvořit záznam DNS hello.

![okno vlastní doménu.](./media/dns-custom-domain/functionshostname.png)

Zóna DNS tooyour přejděte a klikněte na tlačítko **+ sady záznamů**. Vyplňte následující informace na hello hello **přidat sadu záznamů** a klikněte na **OK** toocreate ho.

|Vlastnost  |Hodnota  |Popis  |
|---------|---------|---------|
|Name (Název)     | myfunctionapp        | Tato hodnota společně s hello Popisek názvu domény je hello plně kvalifikovaný název domény pro hello vlastní název domény.        |
|Typ     | CNAME        | Použití záznam CNAME používá alias.        |
|HODNOTA TTL     | 1        | 1 se používá pro 1 hodina        |
|Hodnota TTL jednotky     | Hodiny        | Hodiny se používají jako měření času hello         |
|Alias     | adatumfunction.azurewebsites.NET        | název DNS Hello vytváříte hello alias, v tomto příkladu je název DNS adatumfunction.azurewebsites.net hello poskytované výchozí toohello funkce aplikace.        |

Přejděte zpět tooyour funkce aplikace, klikněte na **funkce**a v části **sítě** klikněte na tlačítko **vlastní domény**, pak v části **názvy hostitelů**klikněte na tlačítko **+ přidat název hostitele**.

Na hello **přidat název hostitele** okno, zadejte hello záznam CNAME v hello **hostname** textové pole a klikněte na tlačítko **ověřením**. Pokud záznam hello bylo možné toobe nalezen, hello **přidat název hostitele** se zobrazí tlačítko. Klikněte na tlačítko **přidat název hostitele** tooadd hello alias.

![funkce aplikace přidat okno název hostitele](./media/dns-custom-domain/functionaddhostname.png)

## <a name="azure-iot"></a>Azure IoT

Azure IoT nemá žádné úpravy, které jsou potřeba na samotnou službu hello. toouse vlastní doménu službou IoT Hub je potřeba pouze záznam CNAME odkazoval toohello prostředky.

Přejděte příliš**Internet věcí** > **IoT Hub** a vyberte své služby IoT hub. Na hello **přehled** okně Poznámka hello plně kvalifikovaný název domény hello IoT hub.

![Okno centra IoT](./media/dns-custom-domain/iot.png)

Dále přejděte tooyour zóny DNS a klikněte na tlačítko **+ sady záznamů**. Vyplňte následující informace na hello hello **přidat sadu záznamů** a klikněte na **OK** toocreate ho.


|Vlastnost  |Hodnota  |Popis  |
|---------|---------|---------|
|Name (Název)     | myiothub        | Tato hodnota společně s hello Popisek názvu domény je hello plně kvalifikovaný název domény pro hello IoT hub.        |
|Typ     | CNAME        | Použití záznam CNAME používá alias.
|HODNOTA TTL     | 1        | 1 se používá pro 1 hodina        |
|Hodnota TTL jednotky     | Hodiny        | Hodiny se používají jako měření času hello         |
|Alias     | adatumIOT.azure devices.net        | název DNS Hello vytváříte hello alias, v tomto příkladu je název hostitele adatumIOT.azure devices.net hello poskytované hello IoT hub.

Po vytvoření záznamu hello otestovat překlad s použitím záznam CNAME hello`nslookup`

## <a name="public-ip-address"></a>Veřejná IP adresa

tooconfigure vlastní doménu pro služby, které používají veřejných IP adres prostředků, jako je například aplikační bránu, Vyrovnávání zatížení, Cloudová služba Správce prostředků virtuálních počítačů, a použít klasické virtuální počítače, záznam CNAME.

Přejděte příliš**sítě** > **veřejnou IP adresu**, vyberte prostředek hello veřejnou IP adresu a klikněte na **konfigurace**. Zaznamenání, zobrazí hello IP adresu.

![okno veřejnou IP adresu](./media/dns-custom-domain/publicip.png)

Zóna DNS tooyour přejděte a klikněte na tlačítko **+ sady záznamů**. Vyplňte následující informace na hello hello **přidat sadu záznamů** a klikněte na **OK** toocreate ho.


|Vlastnost  |Hodnota  |Popis  |
|---------|---------|---------|
|Name (Název)     | mywebserver        | Tato hodnota společně s hello Popisek názvu domény je hello plně kvalifikovaný název domény pro hello vlastní název domény.        |
|Typ     | A        | Použijte záznam prostředku hello je IP adresa.        |
|HODNOTA TTL     | 1        | 1 se používá pro 1 hodina        |
|Hodnota TTL jednotky     | Hodiny        | Hodiny se používají jako měření času hello         |
|IP adresa     | <your ip address>       | Hello veřejnou IP adresu.|

![Vytvoření záznamu a.](./media/dns-custom-domain/arecord.png)

Po vytvoření záznamu hello A spustit `nslookup` přeloží toovalidate hello záznamu.

![vyhledávání dns veřejné IP adresy](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>Služby App Service (webové aplikace)

Hello následující kroky vás provedou konfigurací vlastní doménu pro webové aplikace app service.

Přejděte příliš**Web a mobilní** > **služby App Service** a vyberte hello prostředek, který konfigurujete vlastní název domény a klikněte na tlačítko **vlastní domény**.

Poznámka: adresy url aktuální hello na hello **vlastní domény** okně tato adresa se používá jako hello alias vytvořit záznam DNS hello.

![Okno vlastních domén](./media/dns-custom-domain/url.png)

Zóna DNS tooyour přejděte a klikněte na tlačítko **+ sady záznamů**. Vyplňte následující informace na hello hello **přidat sadu záznamů** a klikněte na **OK** toocreate ho.


|Vlastnost  |Hodnota  |Popis  |
|---------|---------|---------|
|Name (Název)     | mywebserver        | Tato hodnota společně s hello Popisek názvu domény je hello plně kvalifikovaný název domény pro hello vlastní název domény.        |
|Typ     | CNAME        | Použití záznam CNAME používá alias. Pokud prostředek hello používá IP adresu, se použije záznam.        |
|HODNOTA TTL     | 1        | 1 se používá pro 1 hodina        |
|Hodnota TTL jednotky     | Hodiny        | Hodiny se používají jako měření času hello         |
|Alias     | WebServer.azurewebsites.NET        | název DNS Hello vytváříte hello alias, v tomto příkladu je název DNS webserver.azurewebsites.net hello poskytované výchozí toohello webové aplikace.        |


![Vytvořte záznam CNAME](./media/dns-custom-domain/createcnamerecord.png)

Přejděte zpět toohello aplikační služby, který je nakonfigurován pro hello vlastní název domény. Klikněte na tlačítko **vlastní domény**, pak klikněte na tlačítko **názvy hostitelů**. záznam CNAME hello tooadd jste vytvořili, klikněte na tlačítko **+ přidat název hostitele**.

![Obrázek 1](./media/dns-custom-domain/figure1.png)

Po dokončení procesu hello spustit **nslookup** funguje překlad toovalidate.

![Obrázek 1](./media/dns-custom-domain/finalnslookup.png)

toolearn Další informace o mapování tooApp vlastní doménu služby, navštivte [mapovat existující vlastní DNS název tooAzure webové aplikace](../app-service-web/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Pokud potřebujete toopurchase vlastní doménu, navštivte [koupit vlastní název domény pro Azure Web Apps](../app-service-web/custom-dns-web-site-buydomains-web-app.md) toolearn více informací o doménách služby App Service.

## <a name="blob-storage"></a>Blob Storage

Hello následující kroky vás provedou konfigurací záznam CNAME pro účet úložiště blob pomocí metody asverify hello. Tato metoda zajišťuje, že nedojde k žádnému výpadku.

Přejděte příliš**úložiště** > **účty úložiště**, vyberte svůj účet úložiště a klikněte na **vlastní domény**. Zaznamenání, hello plně kvalifikovaný název domény v kroku 2, tato hodnota se používá první záznam CNAME toocreate hello

![objekt BLOB úložiště vlastní domény](./media/dns-custom-domain/blobcustomdomain.png)

Zóna DNS tooyour přejděte a klikněte na tlačítko **+ sady záznamů**. Vyplňte následující informace na hello hello **přidat sadu záznamů** a klikněte na **OK** toocreate ho.


|Vlastnost  |Hodnota  |Popis  |
|---------|---------|---------|
|Name (Název)     | asverify.mystorageaccount        | Tato hodnota společně s hello Popisek názvu domény je hello plně kvalifikovaný název domény pro hello vlastní název domény.        |
|Typ     | CNAME        | Použití záznam CNAME používá alias.        |
|HODNOTA TTL     | 1        | 1 se používá pro 1 hodina        |
|Hodnota TTL jednotky     | Hodiny        | Hodiny se používají jako měření času hello         |
|Alias     | asverify.adatumfunctiona9ed.BLOB.Core.Windows.NET        | název DNS Hello vytváříte hello alias, v tomto příkladu je název DNS asverify.adatumfunctiona9ed.blob.core.windows.net hello poskytované výchozí účet úložiště toohello.        |

Přejděte zpět tooyour účet úložiště kliknutím **úložiště** > **účty úložiště**, vyberte svůj účet úložiště a klikněte na **vlastní domény**. Zadejte alias vytvořen bez předpony asverify hello hello textového pole, zkontrolujte hello ** použít nepřímé ověření CNAME a klikněte na tlačítko **Uložit**. Po dokončení tohoto kroku vrátit tooyour zóny DNS a vytvořit záznam CNAME bez předpony hello asverify.  Po tomto bodě se bezpečné toodelete hello záznam CNAME s předponou cdnverify hello.

![objekt BLOB úložiště vlastní domény](./media/dns-custom-domain/indirectvalidate.png)

Ověřte překlad DNS spuštěním`nslookup`

toolearn najdete informace o mapování koncový bod úložiště objektů blob tooa vlastní domény [konfigurace vlastního názvu doménu pro koncový bod služby Blob storage](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

Hello následující kroky vás provedou konfigurací záznam CNAME pro koncový bod CDN pomocí metody cdnverify hello. Tato metoda zajišťuje, že nedojde k žádnému výpadku.

Přejděte příliš**sítě** > **profilů CDN**vyberte váš profil CDN a klikněte na tlačítko **koncové body** pod **Obecné**.

Vybrat koncový bod hello pracujete s a klikněte na tlačítko **+ vlastní domény**. Poznámka: hello **hostitele koncového bodu** jako tato hodnota je záznam hello záznamů CNAME hello odkazuje na.

![Vlastní domény CDN](./media/dns-custom-domain/endpointcustomdomain.png)

Zóna DNS tooyour přejděte a klikněte na tlačítko **+ sady záznamů**. Vyplňte následující informace na hello hello **přidat sadu záznamů** a klikněte na **OK** toocreate ho.

|Vlastnost  |Hodnota  |Popis  |
|---------|---------|---------|
|Name (Název)     | cdnverify.mycdnendpoint        | Tato hodnota společně s hello Popisek názvu domény je hello plně kvalifikovaný název domény pro hello vlastní název domény.        |
|Typ     | CNAME        | Použití záznam CNAME používá alias.        |
|HODNOTA TTL     | 1        | 1 se používá pro 1 hodina        |
|Hodnota TTL jednotky     | Hodiny        | Hodiny se používají jako měření času hello         |
|Alias     | cdnverify.adatumcdnendpoint.azureedge.NET        | název DNS Hello vytváříte hello alias, v tomto příkladu je název DNS cdnverify.adatumcdnendpoint.azureedge.net hello poskytované výchozí účet úložiště toohello.        |

Přejděte zpět tooyour koncový bod CDN kliknutím **sítě** > **profilů CDN**a vyberte váš profil CDN. Klikněte na tlačítko **+ vlastní domény** zadejte váš alias záznam CNAME bez předpony cdnverify hello a klikněte na tlačítko **přidat**.

Po dokončení tohoto kroku vrátit tooyour zóny DNS a vytvořit záznam CNAME bez předpony cdnverify hello.  Po tomto bodě se bezpečné toodelete hello záznam CNAME s předponou cdnverify hello. Další informace o CDN a jak tooconfigure vlastní domény bez kroku zprostředkující registrace hello navštivte [vlastní doménu CDN Azure mapa obsahu tooa](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Další kroky

Zjistěte, jak příliš[nakonfigurovat zpětné DNS pro služby hostované v Azure](dns-reverse-dns-for-azure-services.md).
