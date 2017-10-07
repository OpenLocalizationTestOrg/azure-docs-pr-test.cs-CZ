---
title: "aaaBuy vlastní název domény pro Azure Web Apps"
description: "Zjistěte, jak toobuy vlastní doménu, název s webovou aplikaci v Azure App Service."
services: app-service\web
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: cephalin
ms.openlocfilehash: 2ff61a3f27020516c917fe105ece99eb2a5754b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Koupit vlastní název domény pro Azure Web Apps

Domény nejvyšší úrovně, které jsou spravovány přímo v Azure jsou domény služby App Service (preview). Provádění ho snadno toomanage vlastní domény [Azure Web Apps](app-service-web-overview.md). Tento kurz ukazuje, jak toobuy domény služby App Service a přiřadit DNS názvy tooAzure webové aplikace.

Tento článek je pro službu Azure App Service (webové aplikace, aplikace API, mobilní aplikace, Logic Apps). Virtuální počítač Azure nebo Azure Storage, najdete v části [přiřadit App Service domény tooAzure virtuálních počítačů nebo úložiště Azure](https://blogs.msdn.microsoft.com/appserviceteam/2017/07/31/assign-app-service-domain-to-azure-vm-or-azure-storage/). Cloudové služby, najdete v části [konfigurace vlastního názvu domény pro cloudové služby Azure](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Požadavky

toocomplete v tomto kurzu:

* [Vytvořit aplikaci služby App Service](/azure/app-service/), nebo použijte aplikaci, kterou jste vytvořili pro jiné kurzu.

## <a name="prepare-hello-app"></a>Příprava aplikace hello

toouse vlastní domény ve službě Azure Web Apps, webové aplikace na [plán služby App Service](https://azure.microsoft.com/pricing/details/app-service/) musí být placené vrstvy (**sdílené**, **základní**, **standardní**, nebo **Premium**). V tomto kroku budete zajistěte, aby že tento hello webové aplikace v hello podporovaná cenová úroveň.

### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure

Otevřete hello [portál Azure](https://portal.azure.com) a přihlaste se pomocí účtu Azure.

### <a name="navigate-toohello-app-in-hello-azure-portal"></a>Přejděte toohello aplikace v hello portálu Azure

Hello levé nabídce vyberte **App Services**a pak vyberte název hello aplikace hello.

![Aplikace tooAzure portálu](./media/app-service-web-tutorial-custom-domain/select-app.png)

Zobrazí stránku hello správu hello aplikaci služby App Service.  

### <a name="check-hello-pricing-tier"></a>Zkontrolujte hello cenové úrovně

V levé navigační stránku hello aplikace hello, posuňte toohello **nastavení** a vyberte **škálování (plán služby App Service)**.

![Škálování nabídky](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

aktuální úroveň aplikace Hello je označený modré ohraničení. Zkontrolujte toomake se, že aplikace hello není hello **volné** vrstvy. Vlastní DNS není podporována v hello **volné** vrstvy. 

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Pokud hello plán služby App Service není **volné**ukončit hello **zvolte cenovou úroveň** stránky a přeskočit příliš[koupit hello domény](#buy-the-domain).

### <a name="scale-up-hello-app-service-plan"></a>Škálování hello plán služby App Service

Vyberte některé z vrstvy a bezplatnou hello (**sdílené**, **základní**, **standardní**, nebo **Premium**). 

Klikněte na **Vybrat**.

![Zkontrolujte cenová úroveň.](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Až uvidíte hello následující oznámení, je dokončena operace škálování hello.

![Potvrzení operace škálování](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-hello-domain"></a>Zakoupení hello domény

### <a name="sign-in-tooazure"></a>Přihlaste se tooAzure
Otevřete hello [portál Azure](https://portal.azure.com/) a přihlaste se pomocí účtu Azure.

### <a name="launch-buy-domains"></a>Spusťte koupit domén
V hello **webové aplikace** , klikněte na název vaší webové aplikaci, vyberte hello **nastavení**a potom vyberte **vlastní domény**
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

V hello **vlastní domény** klikněte na tlačítko **koupit domény**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-hello-domain-purchase"></a>Konfigurace domény nákupu hello

V hello **aplikace služby domény** stránku hello **hledat domény** pole, název domény hello typu chcete toobuy a typ `Enter`. Hello navržené domény k dispozici níže jsou uvedeny pouze hello textové pole. Vyberte jednu nebo více domén, které chcete toobuy. 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

Klikněte na tlačítko hello **kontaktní údaje** a vyplňte formulář hello domény kontaktní informace. Po dokončení klikněte na tlačítko **OK** tooreturn toohello aplikace služby domény stránky.
   
Je důležité, vyplňte všechna povinná pole s tolik přesnost míře. Nesprávná data kontaktní informace může způsobit selhání toopurchase domén. 

V dalším kroku vyberte hello požadované možnosti pro vaši doménu. Viz následující tabulka pro vysvětlení hello:

| Nastavení | Navrhovaná hodnota | Popis |
|-|-|-|
|Automatické obnovení | **Povolení** | Obnovuje doménu služby aplikace automaticky každý rok. Platební karty je účtován hello stejné kupní ceny v době obnovení hello. |
|Ochrana osobních údajů | Povolení | Vyjádřit výslovný souhlas příliš "Ochrany osobních údajů", který je součástí hello kupní ceny _zdarma_ (s výjimkou domény nejvyšší úrovně, jejichž registru nepodporují ochrany osobních údajů, jako například _. co.in_, _. Co.uk_a tak dále). |
| Přiřadit výchozí názvy hostitelů | **Webová** a**@** | Vyberte hello požadované vazby názvů hostitelů, v případě potřeby. Po dokončení operace nákupu domény hello vaší webové aplikace můžete získat přístup na adrese hello vybrané názvy hostitelů. Pokud je webová aplikace hello za [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/), nevidíte hello možnost tooassign hello kořenové domény (@), protože nemá podporu záznamů A Traffic Manager. Můžete provádět změny přiřazení názvu hostitele toohello po dokončení nákupu domény hello. |

### <a name="accept-terms-and-purchase"></a>Přijmout podmínky a nákupu

Klikněte na tlačítko **právní podmínky** tooreview hello podmínky a hello poplatky, pak klikněte na **koupit**.

> [!NOTE]
> Domény služby App Service pomocí Azure DNS toohost hello domén. Kromě toho toohello domény registrace poplatek, poplatky za používání pro Azure DNS použije. Informace najdete v tématu [Azure DNS ceny](https://azure.microsoft.com/pricing/details/dns/).
>
>

Zpět v hello **aplikace služby domény** klikněte na tlačítko **OK**. Když probíhá operace hello, uvidíte hello následující oznámení:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-hello-hostnames"></a>Názvy hostitelů hello testu

Pokud jste přiřadili výchozí názvy hostitelů tooyour webové aplikace, je také zobrazit upozornění na úspěch pro každý vybraný název hostitele. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Zobrazí také hello vybrané názvy hostitelů v hello **vlastní domény** stránku hello **názvy hostitelů** části. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

názvy hostitelů hello tootest, přejděte toohello uvedené názvy hostitelů v prohlížeči hello. V příkladu hello v hello předchozím snímku obrazovky zkuste navigace too_kontoso.net_ a _www.kontoso.net_.

## <a name="assign-hostnames-tooweb-app"></a>Přiřaďte aplikaci tooweb názvy hostitelů

Pokud zvolíte není tooassign jeden nebo více výchozí názvy hostitelů tooyour webovou aplikaci během hello procesu nákupu nebo pokud potřebujete tooassign název hostitele není uvedený, můžete přiřadit název hostitele v kdykoli.

Názvy hostitelů v tooany hello doména aplikace služby lze přiřadit také jiné webové aplikace. Hello kroky závisí na tom, jestli hello doména služby aplikace a webové aplikace hello patří toohello stejného předplatného.

- Jiné předplatné: namapovat vlastní záznamy DNS ze hello aplikace služby domény toohello webová aplikace jako externě zakoupené domény. Informace o přidání vlastní DNS názvy tooan doména služby aplikace, najdete v části [vlastní záznamy DNS spravovat](#custom). toomap externí zakoupené domény tooa webové aplikace, najdete v části [mapovat existující vlastní DNS název tooAzure webové aplikace](app-service-web-tutorial-custom-domain.md). 
- Stejného předplatného: hello použijte následující kroky.

### <a name="launch-add-hostname"></a>Spuštění přidat název hostitele
V hello **App Services** stránky, vyberte hello název webové aplikace, který chcete tooassign názvů hostitelů, vyberte **nastavení**a potom vyberte **vlastní domény**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Ujistěte se, že doménu zakoupené je uvedena ve hello **doménami aplikací služby** tématu, ale nezaškrtnete políčko. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Všechny aplikace služby domény ve hello stejného předplatného se zobrazují v hello webové aplikace **vlastní domény** stránky. Pokud vaše doména je v předplatném hello webové aplikace, ale se nezobrazí v hello webové aplikace **vlastní domény** stránky, zkuste to znovu hello **vlastní domény** stránky nebo aktualizovat webovou stránku hello. Zkontrolujte také, hello oznámení zvonku hello horní části hello portál Azure pro průběh nebo vytvoření chyby.
>
>

Vyberte **přidat název hostitele**.

### <a name="configure-hostname"></a>Nakonfigurujte název hostitele
V hello **přidat název hostitele** dialogové okno, typ hello plně kvalifikovaný název domény vaší domény služby aplikace nebo jakékoli subdomény. Například:

- kontoso.NET
- www.kontoso.NET
- ABC.kontoso.NET

Po dokončení vyberte **ověřením**. Typ záznamu Hello název hostitele je automaticky vybrán za vás.

Vyberte **přidat název hostitele**.

Po dokončení operace hello zobrazí upozornění na úspěch hello přiřazen název hostitele.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Zavřete přidat název hostitele
V hello **přidat název hostitele** přiřaďte žádné jiné hostname tooyour webové aplikace, podle potřeby. Po dokončení zavřete hello **přidat název hostitele** stránky.

Teď byste měli vidět hello nově přiřazeny hostname(s) ve vaší aplikaci **vlastní domény** stránky.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-hello-hostnames"></a>Názvy hostitelů hello testu

Přejděte toohello uvedené názvy hostitelů v prohlížeči hello. V příkladu hello v předchozím snímku obrazovky hello zkuste navigace too_abc.kontoso.net_.

<a name="custom" />

## <a name="manage-custom-dns-records"></a>Spravovat vlastní záznamy DNS

V Azure, záznamy DNS pro doménu služby App Service se spravují pomocí [Azure DNS](https://azure.microsoft.com/services/dns/). Můžete přidat, odebrat a aktualizovat záznamy DNS, stejně jako pro externě zakoupené doménu.

### <a name="open-app-service-domain"></a>Otevřete aplikaci služby domény

V hello portál Azure, hello levé nabídce, vyberte **více služeb** > **doménami aplikací služby**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Vyberte domény toomanage hello. 

### <a name="access-dns-zone"></a>Zóna DNS přístup

V levé nabídce hello domény, vyberte **zónu DNS**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Tato akce otevře hello [zónu DNS](../dns/dns-zones-records.md) stránku vaší domény služby aplikace v Azure DNS. Informace o tom tooedit záznamy DNS, najdete v části [jak hello toomanage zóny DNS v portálu Azure](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Zrušit nákup (odstranit domény)

Po zakoupení hello aplikace služby domény máte pět dní toocancel nákupu pro vrátit celou částku. Po pět dní můžete odstranit hello doména služby aplikace ale nemůže přijímat náhrada.

### <a name="open-app-service-domain"></a>Otevřete aplikaci služby domény

V hello portál Azure, hello levé nabídce, vyberte **více služeb** > **doménami aplikací služby**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Vyberte hello domény tooyou chcete toocancel nebo odstranit. 

### <a name="delete-hostname-bindings"></a>Odstranit vazby názvů hostitelů.

V levé nabídce hello domény, vyberte **vazby názvů hostitelů**. vazby názvů hostitelů Hello ze všech služeb Azure jsou zde uvedeny.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Hello aplikace služby domény nelze odstranit, dokud nejsou odstraněny všechny vazby názvů hostitelů.

Odstranit vazbu každý název hostitele tak, že vyberete **...**   >  **Odstranit**. Po odstranění se všechny vazby hello, vyberte **Uložit**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Zrušit nebo odstranění

V levé nabídce hello domény, vyberte **přehled**. 

Pokud nebyl uplynutí období zrušení hello v doméně hello zakoupili, vyberte **zrušit nákup**. Jinak se zobrazí **odstranit** tlačítko místo. toodelete hello domény bez náhrady, vyberte **odstranit**.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Vyberte **OK** tooconfirm hello operaci. Pokud nechcete, aby tooproceed, klikněte kamkoli mimo dialogové okno potvrzení hello.

Po dokončení operace hello hello doména je vydaná ze svého předplatného a k dispozici pro každý, kdo toopurchase znovu. 
