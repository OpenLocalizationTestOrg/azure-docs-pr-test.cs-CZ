---
title: fakturace hello aaaChange Azure Remoteappu | Microsoft Docs
description: "Zjistěte, jak se fakturuje toostop pro Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 8f94da9a-7848-4ddc-b7b7-d9c280ccf4d7
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mbaldwin
ms.openlocfilehash: fe3841a88978ec56829932621489e75d5dd7e673
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-azure-remoteapp-toomycloudit"></a>Migrace z tooMyCloudIT Azure RemoteApp 

**Aktuálně používáte Microsoft Azure RemoteApp?** : MyCloudIT vytvořené toomigrate automatizovaného nástroje správy platformu Azure RemoteApp (ARA) kolekcí toohello MyCloudIT přitom dál toorun v Microsoft Azure.

**Využít výhod portálu Azure Resource Manager hello**: dokončené migrace do platformy MyCloudIT hello umožňuje okamžitý přístup tooAzure nový portál Azure Resource Manager. Tento portál obsahuje všechny hello nové funkce a inovací, které nabízí Microsoft Azure, včetně přístupu tooVirtual počítač velikosti tooensure vašeho nasazení je sestavena toosupport hello potřebám vaší firmy.

**Testování v paralelní tooensure hello správným řešením pro vaše potřeby**: nástroj pro migraci MyCloudIT hello vychází proces migrace hello tooinitiate a testovací paralelní chvíli vaše aktuální ARA uživatelé dál toouse ARA.  Vaši uživatelé zůstane v ARA dokud migrace a testování jsou dokončeny.  Nástroj pro migraci Hello vychází toohandle hello typické ARA kolekce.  Pokud si myslíte, že máte scénáři jedinečný, nestandardní, obraťte se na nás na adrese [ sales@conexlink.com ](mailto:sales@conexlink.com) proto poskytujeme tooassist šité na míru plán s migrací.

**Možnosti plochy jako služby**: Upozorňujeme, že MyCloudIT nejen nabízí možnosti vzdálené aplikace RemoteApp hello jste zvyklí, ale jsme úplné Desktop-jako-A-Service také nabízí možnosti pro hello stejné náklady za měsíc bez jakékoli minimální uživatele požadavky.

## <a name="what-we-will-do-for-you"></a>Co jsme se to pro vás

MyCloudIT automatizuje hello migrace šablony Azure RemoteApp z portálu Azure Classic hello vaše předplatné toohello portálu správce prostředků Azure svého předplatného s naše automatizované migrační nástroj.  

> [!NOTE]
> hello Hello Azure RemoteApp šablony musí zůstat ve stejné oblasti Azure jako původní nasazení Azure RemoteApp.  Pokud potřebujete během migrace hello toochange Azure oblastí nebo předplatných Azure, kontaktujte nás pro další pokyny v [ sales@conexlink.com ](mailto:sales@conexlink.com).

Přečtěte si níže podrobné informace o procesu migrace hello automatizované s hello MyCloudIT nástroj pro migraci:

1. Nástroj pro migraci Hello kontroluje váš aktuální odběry pro všechna existující nasazení ARA.  
2. Vyberte jednu kolekci toomigrate ARA najednou.  Pokud máte více kolekcí, můžete spusťte modul snap-in nástroje společnosti vícekrát.
3. Máte hello možnost toocopy hello disky profilu v uživatele (UPD) tooyour nové nasazení, můžete buď načíst data starší verze, nebo ručně mapování UPD toohello nové nasazení. Pokud si zvolíte toocopy vaše UPD, jsme uložíte hello UPD a zahrnují textový soubor, který mapuje název hello UPD název tooeach uživatelů.  Hello UPD bude zkopírovaný tooa sdílené složky na serveru RDSMGMT hello `F:\Shares\LegacyUPD` a bude zpřístupnit přes sdílenou složku hello `\\RDSmgmt\LegacyUPD`. 
4. Migrace bude pro vaše aktuální nasazení ARA vyžadovat nedojde k žádnému výpadku.  Ale pokud se udělají jakékoli změny toohello UPD (z ARA) po kopírování hello, tyto změny se neprojeví v hello UPD uložené v portálu Azure Resource Manager hello. 
5. Pokud máte další virtuální počítače jako řadiče domény a souborových serverů v klasickou virtuální síť Azure se naváže partnerský vztah mezi existující klasické virtuální sítě Azure VNet a hello nové virtuální sítě jsme pro vás vytvořil, v hello nového prostředku Azure Správce portálu.
6. Naše automatické řešení bude navázat partnerský vztah mezi existující klasické virtuální sítě Azure VNet a hello nové virtuální sítě, pokud je vaše stávající nasazení ARA hybridní nasazení; znamená, se ověřují pomocí systému Windows Server Active Directory řadiče domény v hello existující klasické virtuální sítě. Pokud není naváže partnerský vztah pro vaše kolekce virtuální sítě, ale budete potřebovat virtuální síť partnerský vztah, kontaktujte nás jako [ sales@conexlink.com ](mailto:sales@conexlink.com) a budeme radostí tooconfigure VNet peering bez dalších poplatků.
7. Naše automatizované řešení zajistí konfiguraci Azure DNS se aktualizuje pomocí hello tooensure nastavení nové virtuální sítě můžete připojit nové nasazení tooyour existující řadič domény v hello klasické virtuální sítě.
8. Naše automatické řešení bude zajistěte, aby žádné ke konfliktům IP adres jako vytvoření této nové virtuální sítě a vytvořit hello partnerský vztah pro nasazení, která máte existující Windows Server Active Directory Server virtuální sítě.
9. Pokud používáte pouze Azure AD pro ověřování, MyCloudIT vytvoří nové domény systému Windows Server Active Directory a pomocí uživatele Azure AD Connect toosynchronize mezi hello existující instanci Azure AD a hello vytvořit nové doméně systému Windows Server Active Directory podle MyCloudIT.
10. Pokud používáte uživatelů systému Windows Server Active Directory Domain tooauthenticate ARA, naše automatizované řešení se připojí vaše nové tooyour nasazení MyCloudIT existující řadič pro doménu Active Directory systému Windows Server prostřednictvím partnerského vztahu virtuální sítě.
11. Pokud používáte Azure Active Directory Domain Services pro ověřování, jsme můžete migrovat, ale kontaktujte nás prosím, takže jsme pro vás vytvořil plán vlastní migrace.  Odešlete e-mail příliš[sales@conexlink.com](mailto:sales@conexlink.com). 
12. Po migraci kolekce toobe hello je potvrzen, sledujte a uvolnit při naše automatizované řešení migruje vaše kolekce ARA a disky profilu uživatele (volitelné) toohello nové vzdálené aplikace MyCloudIT řízené řešení.
13. Po dokončení nasazení hello jsme bude znovu publikovat hello stejné aplikace, které byly publikované v ARA a po nasazení budete moct toopublish další aplikace.

## <a name="post-migration-benefits"></a>Po migraci výhody

1. Poskytujeme Konzola pro správu hello, která vám umožní toomanage hello úplný životní cyklus nasazení vzdálené aplikace.
2. Bude moct toomanage vaše virtuální počítače z náš portál.  Spuštění / zastavení a změně velikosti jednotlivých virtuálních počítačů, v případě potřeby.
3. Konzola pro správu Hello poskytuje hello možnost toocreate a spravujte uživatele / skupiny z portálu pro správu.
4. Konzola pro správu Hello poskytuje uživatelům toosynchronize možnost hello toocreate Office 365 stejné prostředí pro přihlašování.
5. Hello Konzola pro správu poskytuje možnost toocreate hello další vzdálené aplikace a kolekce plochy bez nákladů duplicitního uživatele nebo požadavky na minimální uživatele. 
6. Konzola pro správu Hello poskytuje možnost hello toopublish nové aplikace vzdálené aplikace.
7. Konzola pro správu Hello poskytuje hello možnost tooschedule hello spuštění a vypnutí nasazení vzdálené aplikace, pokud potřebujete řešení pouze během určité doby.
8. Konzola pro správu Hello poskytuje hello možnost tooautomate hello instalaci a konfiguraci agenta Azure Backup hello, který poskytuje historii uchovávání dokumentu zákaznická data.
9. Konzola pro správu Hello poskytuje metriky tooperformance přístup vašeho nasazení.  Tato poskytuje vám možnost tooidentify hello všechny potenciální problémová místa výkonu bez instalace nástroje pro správu Další výkonu.
10. Pokud máte několik hostitelů relace, bude možné tooenable automatické škálování tak pouze hello relace, které jsou spuštěné hostitele, kteří potřebují toobe systémem.
11. MyCloudIT poskytuje server brány RDWeb toohello přístupu prostřednictvím MyCloudIT název domény.  Poskytujeme také hello možnost toore mapy hello URL tooa vlastní adresu URL pro koncového uživatele brandingu.

## <a name="prerequisites-for-migration"></a>Požadavky pro migraci

1. Musíte mít přístup toohello předplatné Azure, který je hostitelem vaše aktuální řešení Azure RemoteApp.
2. Musí udělit naše portálu oprávnění v rámci vašeho předplatného toomigrate šablony a toocreate nebo upravit nové MyCloudIT nasazení.
3. Upozorňujeme, že kvůli tooa omezení v Azure Remote App, každá kolekce je možné migrovat pouze třikrát.  Pokud potřebujete více než třikrát toomigrate kolekci, můžete zvýšit tooincrease tooAzure lístku export spočítat, nebo kontaktujte nás a jsme vám pomůže při hello ARA požadavek tooincrease hello export count.

## <a name="mycloudit-billing"></a>MyCloudIT fakturace

Najdete v tématu [MyCloudIT ceny pro vzdálené aplikace RemoteApp řešení](https://mcitdocuments.blob.core.windows.net/terms/MyCloudIT_Pricing_Overview.pdf) (PDF) informace o tom toopredict a spravovat náklady na celkový Azure.

Pokud máte dotazy, kontaktujte nás na [ sales@conexlink.com ](mailto:sales@conexlink.com) nebo přehrát video úplnou ukázku hello [Azure RemoteApp migrační nástroj - MyCloudIT](https://www.youtube.com/watch?v=YQ_1F-JeeLM&t=482s). 

