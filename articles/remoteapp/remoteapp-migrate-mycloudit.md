---
title: "Změnit fakturace pro Azure Remoteappu | Microsoft Docs"
description: "Postup zastavení se účtují pro Azure RemoteApp."
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
ms.openlocfilehash: 32fc673eeef01e80c73375bf264206beea8cfbe5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/29/2017
---
# <a name="migrate-from-azure-remoteapp-to-mycloudit"></a>Migrace z Azure RemoteApp na MyCloudIT 

**Aktuálně používáte Microsoft Azure RemoteApp?** : MyCloudIT vytvořené automatizovaného nástroje k migraci kolekcí vaší Azure RemoteApp (ARA) do platformy pro správu MyCloudIT můžete nadále spouštět v Microsoft Azure.

**Využít výhod portálu Azure Resource Manager**: dokončené migrace do platformy pro MyCloudIT umožňuje okamžitý přístup k Azure nový portál Azure Resource Manager. Tento portál obsahuje nové funkce a inovací, které nabízí Microsoft Azure, včetně přístupu k velikosti virtuálních počítačů, aby vaše nasazení je vytvořená tak, aby podporovala potřeby vašeho podniku.

**Test paralelně zajistit správné řešení pro vaše potřeby**: nástroj pro migraci MyCloudIT vychází zahájíte proces migrace a testovací paralelní chvíli nadále používat ARA aktuální ARA uživatelům.  Vaši uživatelé zůstane v ARA dokud migrace a testování jsou dokončeny.  Nástroj pro migraci byl vytvořen, aby zpracování typické ARA kolekce.  Pokud si myslíte, že máte scénáři jedinečný, nestandardní, obraťte se na nás na adrese [ sales@conexlink.com ](mailto:sales@conexlink.com) proto poskytujeme šité na míru plán vám pomůže při migraci.

**Možnosti plochy jako služby**: Upozorňujeme, že MyCloudIT nejen nabízí možnosti vzdálené aplikace RemoteApp jste zvyklí, ale jsme úplné Desktop-jako-A-Service také nabízí možnosti pro stejné náklady za měsíc bez požadavků na minimální uživatele.

## <a name="what-we-will-do-for-you"></a>Co jsme se to pro vás

MyCloudIT automatizuje migrace šablony Azure RemoteApp na portálu Azure Classic předplatného k portálu Azure Resource Manager předplatného nástrojem naše automatizované migrace.  

> [!NOTE]
> Šablony Azure RemoteApp musí zůstat ve stejné oblasti Azure jako původní nasazení Azure RemoteApp.  Pokud potřebujete změnit Azure oblastí nebo předplatných Azure během migrace, kontaktujte nás pro další pokyny v [ sales@conexlink.com ](mailto:sales@conexlink.com).

Podrobné informace o automatické migraci pomocí nástroje Migrace MyCloudIT přečtěte si níže:

1. Nástroj pro migraci kontroluje váš aktuální odběry pro všechna existující nasazení ARA.  
2. Vyberte jeden ARA kolekci pro migraci v čase.  Pokud máte více kolekcí, můžete spusťte modul snap-in nástroje společnosti vícekrát.
3. Máte možnost zkopírovat disky profilu uživatele (UPD) do vaší nové nasazení, můžete buď načíst data starší verze, nebo ručně mapování vaše UPD nové nasazení. Pokud zvolíte možnost Kopírovat vaší UPD, jsme uložíte UPD a zahrnují textový soubor, který mapuje název UPD na název jednotlivých uživatelů.  UPD budou zkopírovány do sdílené složky na serveru RDSMGMT `F:\Shares\LegacyUPD` a bude zpřístupnit přes sdílenou složku `\\RDSmgmt\LegacyUPD`. 
4. Migrace bude pro vaše aktuální nasazení ARA vyžadovat nedojde k žádnému výpadku.  Ale pokud UPD (z ARA) jsou po kopírování udělají jakékoli změny, tyto změny se neprojeví v UPD uložené na portálu Azure Resource Manager. 
5. Pokud máte další virtuální počítače jako řadiče domény a souborové servery v klasickou virtuální síť Azure Navážeme VNet peering – mezi existující klasické virtuální sítě Azure a nové virtuální sítě, které jsou pro vás, vytvoříme nové portálu Azure Resource Manager.
6. Naše automatizované řešení bude pouze navázat partnerský vztah mezi existující klasické virtuální sítě Azure a nové virtuální sítě, pokud je vaše stávající nasazení ARA hybridní nasazení; virtuální sítě znamená, se ověřují pomocí systému Windows Server Active Directory řadiče domény v existující klasické virtuální sítě. Pokud není naváže partnerský vztah pro vaše kolekce virtuální sítě, ale budete potřebovat virtuální síť partnerský vztah, kontaktujte nás jako [ sales@conexlink.com ](mailto:sales@conexlink.com) a budeme rádi konfigurace VNet peering – bez dalších poplatků.
7. Naše automatické řešení bude zajištěno, aby že konfiguraci Azure DNS je aktualizován s novým nastavením virtuální sítě k zajištění, že nové nasazení může připojit k řadiči domény existující klasické virtuální sítě.
8. Naše automatické řešení bude zajistěte, aby žádné ke konfliktům IP adres jako vytvoření této nové virtuální sítě a vytvoření partnerského vztahu virtuální sítě pro nasazení, které máte existující Windows Server Active Directory Server.
9. Pokud používáte pouze Azure AD pro ověřování, MyCloudIT vytvoříte nové domény systému Windows Server Active Directory a používat k synchronizaci uživatelů mezi ke stávající instanci Azure AD a nové Windows serveru domény služby Active Directory vytvořené MyCloudIT Azure AD Connect.
10. Pokud používáte k ověřování uživatelů ARA domény Active Directory systému Windows Server, naše automatizované řešení se připojí vaše nové nasazení MyCloudIT existující systému Windows Server Active Directory řadiče domény přes virtuální síť partnerský vztah.
11. Pokud používáte Azure Active Directory Domain Services pro ověřování, jsme můžete migrovat, ale kontaktujte nás prosím, takže jsme pro vás vytvořil plán vlastní migrace.  Pošlete prosím e-mail na [ sales@conexlink.com ](mailto:sales@conexlink.com). 
12. Jakmile je potvrzen kolekce, které chcete migrovat, sledujte a uvolnit při naše automatizované řešení migruje ARA kolekce a disky profilu uživatele (volitelné) do nového MyCloudIT řízené řešení vzdálené aplikace.
13. Po dokončení nasazení jsme znovu publikovat stejné aplikace, které byly publikované v ARA a post nasazení budete moci publikovat další aplikace.

## <a name="post-migration-benefits"></a>Po migraci výhody

1. Poskytujeme Konzola pro správu, která vám umožní spravovat celý životní nasazení vzdálené aplikace.
2. Bude moct spravovat virtuální počítače z náš portál.  Spuštění / zastavení a změně velikosti jednotlivých virtuálních počítačů, v případě potřeby.
3. Konzole pro správu poskytuje možnost vytvářet a spravovat uživatele nebo skupiny z portálu pro správu.
4. Konzola pro správu poskytuje schopnost synchronizaci uživatelů se službami Office 365 vytvořit stejné prostředí pro přihlašování.
5. Konzola pro správu poskytuje možnost vytvářet další vzdálené aplikace a kolekce plochy bez nákladů duplicitního uživatele nebo požadavky na minimální uživatele. 
6. Konzola pro správu poskytuje možnost publikovat nové aplikace vzdálené aplikace.
7. Konzola pro správu poskytuje schopnost naplánovat spuštění a vypnutí nasazení vzdálené aplikace, pokud potřebujete řešení pouze během určité doby.
8. Konzola pro správu poskytuje možnost automatizovat instalaci a konfiguraci agenta Azure Backup, který poskytuje historii uchovávání dokumentu zákaznická data.
9. Konzole pro správu poskytuje přístup k metriky výkonu vašeho nasazení.  To vám umožňuje identifikovat všechny potenciální problémová místa výkonu bez instalace nástroje pro správu Další výkonu.
10. Pokud máte několik hostitelů relace, bude možné povolit automatické škálování, protože pouze relace, které jsou spuštěné hostitele, kteří potřebují, aby byl spuštěn.
11. MyCloudIT poskytuje přístup k serveru brány RDWeb prostřednictvím MyCloudIT název domény.  Poskytujeme také možnost znovu mapování adresy URL do vlastní adresu URL pro koncového uživatele brandingu.

## <a name="prerequisites-for-migration"></a>Požadavky pro migraci

1. Musíte mít přístup k předplatnému Azure, který je hostitelem vaše aktuální řešení Azure RemoteApp.
2. V rámci vašeho předplatného k migraci šablony a k vytvoření / změnit nové nasazení MyCloudIT musí udělit oprávnění naše portálu.
3. Pamatujte, že kvůli omezení v Azure Remote App, každá kolekce je možné migrovat pouze třikrát.  Pokud potřebujete k migraci kolekce častěji než třikrát, můžete vyvolat lístek k Azure a zvýšit export spočítat, nebo kontaktujte nás a jsme vám pomůže při ARA žádost o zvýšení počtu export.

## <a name="mycloudit-billing"></a>MyCloudIT fakturace

Najdete v tématu [MyCloudIT ceny pro vzdálené aplikace RemoteApp řešení](https://mcitdocuments.blob.core.windows.net/terms/MyCloudIT_Pricing_Overview.pdf) (PDF) informace o tom, jak předpovědi a spravovat náklady na celkový Azure.

Pokud máte dotazy, kontaktujte nás na [ sales@conexlink.com ](mailto:sales@conexlink.com) nebo videu úplnou ukázku [Azure RemoteApp migrační nástroj - MyCloudIT](https://www.youtube.com/watch?v=YQ_1F-JeeLM&t=482s). 

