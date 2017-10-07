---
title: "aaaFAQ pro databáze ClearDB MySql službou Azure App Service | Microsoft Docs"
description: "Odpovědi na otázky toocommon o používání databáze ClearDB MySQL službou Azure App Service."
documentationcenter: php
services: 
author: sunbuild
manager: yochayk
editor: 
tags: mysql
ms.assetid: c2ed5e78-6d7d-4d0c-b7ee-a52ae41ceab8
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2016
ms.author: sumuth
ms.openlocfilehash: 3d9c9daca2b845ede8d3a1fdadefa7e668d62dee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="faq-for-cleardb-mysql-databases-with-azure-app-service"></a>Nejčastější dotazy k databázím ClearDB MySQL ve službě Azure App Service
Tyto nejčastější dotazy odpovědi na běžné dotazy týkající se použití a nákup databáze MySQL cleardb – pro webové aplikace Azure.

## <a name="what-options-do-i-have-for-mysql-on-azure"></a>Jaké možnosti jsou pro databázi MySQL v Azure?
Máte několik možností:

* [Databáze MySQL cleardb – sdílené](/marketplace/partners/cleardb/databases/)
* [ClearDB MySQL Premium clustery](/marketplace/partners/cleardb-clusters/cluster/)
* [Cluster MySQL běžící na virtuálním počítači Azure](https://github.com/azure/azure-quickstart-templates/tree/master/mysql-replication)
* [Jedna instance databáze MySQL spuštěna na virtuálním počítači Azure](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

ClearDB je MySQL, který je hostitelem služby a spravuje hello MySQL infrastruktury pro vás. Při spouštění vlastní MySQL cluster nebo databáze na virtuální počítač Azure, můžete mít tooset server hello MySQL a ponechat ji aktualizovat pomocí opravy.

## <a name="do-i-need-a-credit-card-for-hello-web-app--mysql-template-in-hello-azure-marketplace"></a>Potřebuji platební karty pro webové aplikace hello + šablona MySQL v Azure Marketplace hello?
To závisí na typu hello předplatné, které používáte. Zde jsou některé typy běžně používané předplatného:

* [Platím průběžně](/offers/ms-azr-0003p/): vyžaduje platební kartu, a při nákupu placené databáze MySQL je účtován platební karty.
* [Bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/): obsahuje kredity pro použití se službou Microsoft Azure, služby, ale neumožňuje nákup prostředků třetích stran. toopurchase služeb třetích stran nebo placené databáze MySQL, budete potřebovat toouse platební karty povolené předplatné. Pro webové aplikace můžete vytvořit databázi volné ClearDB MySQL.
* [Předplatné MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/) a **MSDN pro vývoj testování platím průběžně**: podobné tooFree zkušební předplatné MSDN vyžaduje, abyste toohave platební karty toopurchase placené řešení MySQL z ClearDB.
* [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/): EA zákazníků se účtují před jejich EA každé čtvrtletí pro všechny svůj nákup Azure Marketplace (třetí strany) na samostatné, konsolidované faktury. Fakturuje se mimo hello peněžních závazků pro všechny nákupy marketplace. Upozorňujeme, že v tomto okamžiku Azure úložiště není k dispozici toocustomers zaregistrovaná Ázerbájdžán, Chorvatska, Norsko a Portoriko. 
* [DreamSpark](https://www.dreamspark.com/Product/Product.aspx?productid=99): je možné vytvořit jenom volné ClearDB databází pro webové aplikace. Neexistuje žádné omezení počtu hello bezplatné databáze ClearDB MySQL databází, které lze vytvořit. Všimněte si, že bezplatných databází nejsou toobe používá pro provozní webové aplikace, protože tato služba je určena pouze pro zkušební verzi.

## <a name="why-was-i-charged-350-for-a-web-app--mysql-from-hello-azure-marketplace"></a>Proč se mi strhla platba 3.50 $ pro webové aplikace + MySQL z hello Azure Marketplace?
možnost databáze výchozí Hello je Titan, což je $3.50. Jsme nezobrazovat hello náklady během vytváření databáze a můžete zakoupit databázi, kterou jste neměli v úmyslu omylem. Snažíme toofind způsob tooimprove hello zkušeností, ale do té doby je nutné před kliknutím na tlačítko Zkontrolovat všechny vaše vybrané cenové úrovně pro webovou aplikaci a databázi **vytvořit** a spuštění nasazení hello hello prostředků.

## <a name="i-am-running-mysql-on-my-own-azure-virtual-machine-can-i-connect-my-azure-web-app-toomy-database"></a>Používám MySQL na vlastní virtuální počítač Azure. Můžete připojit k službě Azure web app toomy databáze?
Ano. Tak dlouho, dokud svého virtuálního počítače Azure udělil vzdáleného přístupu tooyour webové aplikace se můžete připojit databázi tooyour webové aplikace. Další informace najdete v tématu [nainstalovat MySQL na virtuálním počítači](virtual-machines/windows/classic/mysql-2008r2.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="in-which-countries-are-cleardb-premium-mysql-clusters-supported"></a>Ve kterém jsou jednotlivé země clustery ClearDB Premium MySQL podporované?
[Databáze MySQL ClearDB Premium clustery](/marketplace/partners/cleardb-clusters/cluster/) jsou k dispozici ve všech oblastech Azure po celém světě s výjimkou hello Indie, Austrálie, Brazílie – jih a Číny.

## <a name="can-i-create-a-new-cluster-prior-toocreating-a-database-with-cleardb-premium-cluster-solution"></a>Je možné vytvořit nové předchozí toocreating clusteru databázi ClearDB premium clusteru řešení?
Ne, vytváření clusterů prázdný ClearDB není podporován. Hello portál Azure vám umožní toocreate databází v clusteru, který může vytvoření nového clusteru na hello stejnou dobu.

## <a name="will-i-be-warned-if-i-try-toodelete-a-cleardb-mysql-database-that-is-in-use-by-one-of-my-applications"></a>Zobrazí I upozornění, pokud pokouším toodelete databáze MySQL cleardb –, která je používána jedním z mé aplikace?
Ne, Azure nebude varovat, pokud je odstranit nákupu marketplace, které závisí aplikace na.

## <a name="which-regions-can-i-create-cleardb-databases-in"></a>Oblasti, které lze vytvořit ClearDB databází v?
Azure Marketplace není k dispozici toocustomers registrovaná v Ázerbájdžán, Chorvatsko, Norsko nebo Portoriko. ClearDB není k dispozici v těchto oblastech.

## <a name="what-pricing-tier-should-i-choose-for-a-production-web-app-and-database"></a>Jaké cenová úroveň by měl vybrat pro provozní webové aplikace a databáze?
Použijte základní nebo používat vyšší cenová úroveň pro webové aplikace. Pro cleardb – doporučujeme, abyste buď Saturn nebo Jupiter plán. Zkontrolujte hello funkce a omezení jednotlivých cenových úrovní pro obě [webové aplikace](https://azure.microsoft.com/pricing/details/app-service/) a [databáze ClearDB MySQL](/marketplace/partners/cleardb/databases/) hello toochoose, ten, který vyhovuje vašim potřebám.

## <a name="how-do-i-upgrade-my-cleardb-database-from-one-plan-tooanother"></a>Jak upgradovat databázi ClearDB z jednoho plánu tooanother?
V hello [portál Azure](https://portal.azure.com), můžete postupně škálovat sdílenou databázi ClearDB hostování. Přečtěte si to [článku](https://blogs.msdn.microsoft.com/appserviceteam/2016/10/06/upgrade-your-cleardb-mysql-database-in-azure-portal/) toolearn Další. Aktuálně nepodporujeme aktualizace pro clustery ClearDB Premium v hello portálu Azure.

## <a name="i-cant-see-my-cleardb-database-in-azure-portal"></a>Nelze zobrazit databázi ClearDB na portálu Azure?
Pokud jsme vytvořit databázi ClearDB pomocí Azure Resource Manager nebo [nový portál Azure](https://portal.azure.com), nebude se zobrazovat v hello [starý portál Azure](https://manage.windowsazure.com). toowork-řešení je toolink databázi ručně toohello webové aplikace. Podobně pokud vytvořit databázi ClearDB v hello [starý portál](https://manage.windowsazure.com) není budete mít možnost toosee vaše databáze v hello [nový portál Azure](https://portal.azure.com). Neexistuje žádné řešení pro pozdější scénář hello.

## <a name="who-do-i-contact-for-support-when-my-database-is-down"></a>Kdo I požádat o podporu Pokud databáze je dolů?
Obraťte se na [ClearDB podporu](https://www.cleardb.com/developers/help/support) pro všechny databáze související problémy. Připravit tooprovide je s informace o vašem předplatném Azure.

## <a name="can-i-create-additional-users-for-my-cleardb-mysql-database-cluster-solution"></a>Můžete vytvořit další uživatele pro moje řešení clusteru databáze ClearDB MySQL?
Ne. Nelze vytvořit další uživatele, ale můžete vytvořit další databáze v databázi ClearDB clusteru.  

## <a name="can-basicpro-series-databases-be-upgraded-in-place-similar-tooplanetary-plans-today-on-cleardb-portal"></a>Může být databáze Basic/Pro řady upgradované na místě podobné tooPlanetary plány dnes na portálu ClearDB?
Ano, základní řady, který může být databáze upgradovat na místě (základní 60 prostřednictvím základní 500). Pro řadu lze upgradovat na místě (Pro 125 prostřednictvím Pro 1000) s výjimkou Pro 60. Upgrade databáze Pro 60 aktuálně nepodporujeme. 

## <a name="when-i-migrate-my-resources-from-one-subscription-tooanother-does-my-cleardb-mysql-database-get-migrated-as-well"></a>Když migrovat Moje prostředky z jedné tooanother předplatné, databáze MySQL cleardb – migrována také?
Když provádíte migraci prostředků ve předplatných, některé [omezení](app-service-web/app-service-move-resources.md) použít. Databáze ClearDB MySQL je službu třetí strany a proto nejsou migrována během migrace předplatného Azure. Pokud nespravujete hello migrace vaší MySQL databáze předchozí toomigrating Azure prostředky, vaše MySQL cleardb – databáze je možné zakázat. Nejprve ručně migrovat své databáze a pak proveďte migraci předplatného Azure pro vaši webovou aplikaci. 

## <a name="i-hit-hello-spending-limit-on-my-subscription-i-removed-hello-limit-and-my-app-service-is-online-however-hello-database-is-not-accessible-how-do-i-re-enable-hello-cleardb-database"></a>Dosáhnu hello útrat na Moje předplatné. Po odebrání hello limit a Moje App Service je online, ale hello databáze není přístupná. Jak znovu povolit databázi ClearDB hello?
Obraťte se na [ClearDB podporu](https://www.cleardb.com/developers/help/support) toore povolit hello databáze. Zadejte pro ně vašeho předplatného Azure informace a název databáze.

## <a name="can-i-transfer-a-cleardb-database-from-a-credit-card-subscription-tooan-ea-subscription"></a>Můžete převést databázi ClearDB z platební karty předplatné tooan EA předplatné?
Existující databáze ClearDB používají hello platební karty spojené s existující odběry hello. toouse předplatné EA potřebujete toomigrate nové databáze tooa dat:

* Kupte si novou databázi ClearDB s předplatným EA.
* Migrace dat tooyour nové databáze.
* Aktualizujte aplikaci toouse hello nové databáze.
* Odstraňte starou databázi ClearDB.

Když vytvoříte novou webovou aplikaci s MySQL (cleardb –) nebo vytvoření databáze MySQL cleardb (–), hello předplatné, které určuje, jak bude platit pro službu hello. EA předplatné jsme neblokuje hello zprostředkování hello služby třetích stran, jako je například ClearDB v hello portálu Azure. Odběry EA se účtují mimo peněžních závazků a fakturují se čtvrtletně a zpětně. Hello EA zákazníka by měla mít tooset až platby například toopay platební karty pro všechny služby třetích stran marketplace.

## <a name="where-can-i-see-hello-charges-for-cleardb-resources-in-an-ea-subscription"></a>Kde lze zobrazit hello poplatky za ClearDB prostředků v předplatném služby EA?
Pro zákazníky přímé EA poplatky Azure Marketplace se zobrazí na hello podnikový portál. Všimněte si, že všechny nákupy marketplace a spotřeba se účtují mimo peněžních závazků a fakturují se čtvrtletně a zpětně. Zákazníci, EA mají toopay přímo toohello poskytovatelé služeb třetích stran a může tak povolením platby například platební karty s jejich EA účtu.

Nepřímý zákazníkům EA najdete svých předplatných Azure Marketplace na hello **Spravovat odběry** stránku hello podnikového portálu, ale ceny skryt. Zákazníci získáte jejich LSP informace na webu marketplace poplatky.

Správci registrace EA Azure lze spravovat přístup tooAzure Marketplace pro služby třetích stran. Jejich můžete vypnout nebo zapnout přístup too3rd strany nákupy z hello úložiště v Správa účtů a odběry v oddílu účtů hello hello podnikový portál.

## <a name="who-do-i-contact-for-questions-about-my-bill-for-cleardb-services-in-my-ea-subscription"></a>Kdo I contact pro dotazy o Moje fakturovaná částka u ClearDB služby v Moje předplatné EA?
Obraťte se na [Enterprise zákaznickou podporu](http://aka.ms/AzureEntSupport) s toobilling namapoval pod jejich EA registrace. Hello tým podpory portál EA bude odpovídající vaší otázce nebo váš problém vyřešit.

## <a name="more-information"></a>Další informace
[Nejčastější dotazy k Azure Marketplace](/marketplace/faq/)

