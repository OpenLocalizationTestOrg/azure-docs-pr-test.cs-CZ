---
title: "úlohy správy aaaCommon cloudové služby (klasické) | Microsoft Docs"
description: "Zjistěte, jak toomanage cloudových služeb v hello portál Azure classic."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 41a30e50-067c-485b-96fd-434a6d057abc
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: adegeo
ms.openlocfilehash: 03b1d632f1480d0f65c87b7f8ffc747417acf7b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-cloud-services"></a>Jak tooManage cloudových služeb
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-manage-portal.md)
> * [Portál Azure Classic](cloud-services-how-to-manage.md)
>
>

V hello **cloudové služby** oblasti hello Azure classic portálu, můžete aktualizovat roli služby nebo nasazení, zvýšení úrovně tooproduction dvoufázového nasazení, propojte prostředky tooyour cloudové služby, můžete zobrazit hello prostředky závislosti a škálování hello prostředky společně a odstraňte cloudovou službu nebo nasazení.

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Postupy: aktualizovat roli služby cloudu nebo nasazení
Pokud potřebujete kód aplikace hello tooupdate pro cloudové služby, použijte **aktualizace** na řídicím panelu hello **cloudové služby** stránky, nebo **instance** stránky. Můžete aktualizovat roli jednoho nebo všech rolí. Budete potřebovat tooupload nový balíček služby a konfigurační soubor služby.

1. V hello [portál Azure classic](https://manage.windowsazure.com/), na řídicím panelu hello **cloudové služby** stránky, nebo **instance** klikněte na tlačítko **aktualizace**.

    ![UpdateDeployment](./media/cloud-services-how-to-manage/CloudServices_UpdateDeployment.png)

2. V **označení nasazení**, zadejte název tooidentify hello nasazení (například mycloudservice4). Zjistíte označení nasazení hello pod **úvodní** na řídicím panelu hello.
3. V **balíček**, použijte **Procházet** tooupload hello souboru balíku služeb (.cspkg).
4. V **konfigurace**, použijte **Procházet** tooupload hello služby konfiguračního souboru (.cscfg).
5. V **Role**, vyberte **všechny** Pokud chcete, aby tooupgrade všechny role v hello Cloudová služba. Aktualizovat tooperform jedné role, vyberte hello role, které chcete tooupdate. I když vyberete tooupdate konkrétní roli, hello aktualizace v konfiguračním souboru služby hello jsou použité tooall role.
6. Pokud změny aktualizace hello hello počet rolí nebo hello velikost žádnou roli, vyberte hello **povolit aktualizaci, pokud se změní velikost role nebo počet rolí** tooproceed aktualizace hello tooenable zaškrtávací políčko.

    Mějte na paměti, když změníte velikost hello role (tedy hello velikost virtuálního počítače, který je hostitelem role instance) nebo hello počet rolí, každá instance role (virtuálním počítači) musí být znovu s vytvořenými bitovými kopiemi a všechny místní data budou ztracena.

7. Pokud všechny služby role mít pouze jednu instanci role, vyberte hello **aktualizovat i když jeden nebo více rolí obsahuje jednu instanci zaškrtávací políčko** upgradu tooproceed tooenable hello.

    Azure můžete pouze zaručit 99,95 % dostupnost služby při aktualizaci služby cloud pokud každá role má aspoň dvě instance role (virtuální počítače). Umožňující jeden virtuální počítač tooprocess klientských požadavků v průběhu aktualizace hello jiné.

8. Klikněte na tlačítko **OK** aktualizaci služby hello toobegin (zaškrtnutí).

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a>Postupy: Prohodit nasazení toopromote tooproduction dvoufázového nasazení
Použití **Prohodit** toopromote pracovní nasazení tooproduction cloudové služby. Pokud se rozhodnete toodeploy novou verzi cloudové služby, můžete dvoufázové instalace a otestovat novou verzi v pracovní prostředí vaší cloudové služby, když vaši zákazníci používají hello aktuální verzi v produkčním prostředí. Až budete připraveni toopromote hello tooproduction nové verze, můžete použít **Prohodit** tooswitch hello adresy URL, pomocí které hello se dvě nasazení podrobněji.

Můžete zaměnit nasazení z hello **cloudové služby** stránky nebo hello řídicího panelu.

1. V hello [portál Azure classic](https://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**.
2. V seznamu hello cloudových služeb, klikněte na tlačítko hello cloudové služby tooselect ho.
3. Klikněte na tlačítko **odkládacího souboru**.

    Hello následující potvrzovací výzvu otevře.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage/CloudServices_Swap.png)

4. Po ověření, informace o nasazení hello, klikněte na tlačítko **Ano** tooswap hello nasazení.

    swap nasazení Hello dochází rychle hello jediné, co se změní je hello virtuální IP adresy (VIP) pro nasazení hello.

    náklady na výpočetní toosave, můžete odstranit hello nasazení v hello pracovní prostředí, jakmile se přesvědčíte, že hello nové produkční nasazení funguje podle očekávání.

### <a name="common-questions-about-swapping-deployments"></a>Časté otázky týkající se nasazení vzájemná záměna

**Jaké jsou požadavky hello pro odkládací nasazení?**

Existují dva klíče požadavky pro úspěšné nasazení prohození:

- Pokud chcete pro produkční slot toouse statickou IP adresu, musíte rezervovat jeden pro vaše přípravný slot. Hello odkládacího souboru, jinak nebude úspěšná.

- Všechny instance role musí být spuštěna, než bude možné provést hello swap. Můžete zkontrolovat stav hello vaše instance v hello portál Azure classic nebo pomocí [hello příkaz Get-AzureRole v prostředí Windows PowerShell](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0).

Všimněte si, že aktualizace operačního systému hosta a službou opravy operace může také způsobit toofail záměna nasazení. V tématu [Poradce při potížích se nasazení služby cloud](cloud-services-troubleshoot-deployment-problems.md) další podrobnosti.

**Nesnižuje prohození výpadek své aplikaci? Jak by měly zpracovávat ho?**

Jak je popsáno v poslední části hello, prohození nasazení je obvykle velmi rychlé vzhledem k tomu, že je jenom ke změně konfigurace pro vyrovnávání zatížení Azure hello. V některých případech ale ho můžete trvat deset nebo víc sekund a způsobit selhání přechodný připojení. toolimit dopad tooyour zákazníků, zvažte implementaci [logika opakovaných pokusů klienta](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-tooa-cloud-service"></a>Postupy: odkaz prostředků tooa cloudové služby
tooshow závislosti cloudové služby na jiné prostředky, můžete propojit instance databáze SQL Azure nebo Cloudová služba toohello účet úložiště. Můžete propojit a zrušit propojení prostředky na hello **propojené prostředky** stránky a následně monitorovat jejich využití na řídicí panel hello cloudové služby. Pokud propojený účet úložiště monitorování zapnutý, můžete sledovat celkový počet požadavků na řídicí panel hello cloudové služby.

Použití **odkaz** toolink nový nebo existující databázi SQL instance nebo úložiště účet tooyour cloudové služby. Potom můžete škálovat hello databáze spolu s hello cloudové služby role, která jej používá na hello **škálování** stránky. (Účet úložiště škáluje automaticky jako zvýšení využití.) Další informace najdete v tématu [jak tooScale Cloudová služba a propojené prostředky](cloud-services-how-to-scale.md).

Také můžete monitorovat, spravovat a škálování hello databáze v hello **databáze** uzlu hello portál Azure classic.

"Propojování" prostředků v tomto smyslu není připojit toohello prostředek vaší aplikace. Pokud vytvoříte novou databázi pomocí **odkaz**, budete potřebovat tooadd hello připojovací řetězce tooyour kódu aplikace a pak upgradovat hello cloudové služby. Budete také potřebovat tooadd připojovací řetězce Pokud vaše aplikace používá prostředky v propojený účet úložiště.

Hello následující postup popisuje, jak toolink novou instanci databáze SQL, nasazena na nový server databáze SQL, tooa cloudové služby.

### <a name="toolink-a-sql-database-instance-tooa-cloud-service"></a>toolink SQL Database instance tooa cloudové služby
1. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**. Klikněte na tlačítko hello název řídicího panelu tooopen hello hello cloudové služby.
2. Klikněte na tlačítko **propojené prostředky**.

    Hello **propojené prostředky** otevře se stránka.

    ![LinkedResourcesPage](./media/cloud-services-how-to-manage/CloudServices_LinkedResourcesPage.png)

3. Klikněte na možnost **odkaz prostředek** nebo **odkaz**.

    Hello **prostředků odkaz** spustí se průvodce.

    ![Odkaz Page1](./media/cloud-services-how-to-manage/CloudServices_LinkedResources_LinkPage1.png)

4. Klikněte na tlačítko **vytvořte nový prostředek** nebo **propojení na existující prostředek**.
5. Zvolte typ hello toolink prostředků. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **SQL Database**. (Jenom hello klasický portál Azure podporuje propojení účtu úložiště tooa cloudové služby.)
6. Konfigurace databáze toocomplete hello, postupujte podle pokynů v nápovědě pro hello **databází SQL** oblasti hello portál Azure classic.

    Můžete postupovat podle hello průběh hello propojení operace v oblasti zpráv hello.

    Po dokončení propojení můžete sledovat stav hello hello propojené prostředku na řídicí panel hello cloudové služby. Informace o škálování propojené databáze SQL najdete v tématu [jak tooScale Cloudová služba a propojené prostředky](cloud-services-how-to-scale.md).

### <a name="toounlink-a-linked-resource"></a>toounlink propojeného prostředku
1. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**. Klikněte na tlačítko hello název řídicího panelu tooopen hello hello cloudové služby.
2. Klikněte na tlačítko **propojené prostředky**a potom vyberte hello prostředků.
3. Klikněte na tlačítko **zrušit propojení**. Pak klikněte na tlačítko **Ano** na výzvu k potvrzení hello.

    Databáze SQL se odpojuje nemá žádný vliv na hello databázi nebo databázi toohello aplikace hello připojení. Stále můžete spravovat hello databáze v hello **databází SQL** oblasti hello portál Azure classic.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Postupy: odstranění nasazení a cloudové služby
Před odstraněním cloudové služby, je nutné odstranit každé existující nasazení.

náklady na výpočetní toosave, jakmile ověříte, že produkčního nasazení funguje podle očekávání, můžete odstranit pracovní nasazení. Jste fakturovaná výpočetní náklady pro instance rolí i v případě, že Služba cloudu není spuštěna.

Použijte následující postup toodelete hello nasazení nebo cloudové služby.

1. V hello [portál Azure classic](http://manage.windowsazure.com/), klikněte na tlačítko **cloudové služby**.
2. Vyberte hello cloudové služby a pak klikněte na tlačítko **odstranit**. (tooselect cloudové služby bez otevření hello řídicí panel, klikněte na libovolné místo kromě názvu hello v položku hello cloudové služby.)

    Pokud máte nasazení v pracovním nebo produkčním, zobrazí se nabídky možností podobné toohello následující jednu po hello dolní části okna hello. Před odstraněním hello cloudové služby, je nutné odstranit všechna existující nasazení.

    ![Odstranění nabídky](./media/cloud-services-how-to-manage/CloudServices_DeleteMenu.png)

3. toodelete nasazení, klikněte na tlačítko **odstranit produkční nasazení** nebo **odstranit pracovní nasazení**. Klikněte na výzvy k potvrzení hello **Ano**.
4. Pokud máte v plánu toodelete hello cloudové služby, opakujte krok 3, v případě potřeby toodelete další nasazení.
5. toodelete hello Cloudová služba, klikněte na tlačítko **odstranění Cloudová služba**. Klikněte na výzvy k potvrzení hello **Ano**.

> [!NOTE]
> Pokud podrobné monitorování je nakonfigurovaný pro cloudové služby, Azure hello monitorování data z účtu úložiště při odstranění hello Cloudová služba neodstraní. Toodelete hello dat budete potřebovat ručně. Informace o kde toofind hello metriky tabulky najdete v tématu "postup: přístup k podrobné monitorování data mimo portál Azure classic hello" v [jak tooMonitor cloudových služeb](cloud-services-how-to-monitor.md).
>
>

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure.md).
* Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate.md).
