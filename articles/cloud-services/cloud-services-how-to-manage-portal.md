---
title: "úlohy správy služby cloud aaaCommon | Microsoft Docs"
description: "Zjistěte, jak toomanage cloudových služeb v hello portálu Azure. Tyto příklady použití hello portálu Azure."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: cb218ad9-77d4-4149-83db-71159c00767e
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: ade8a18a7754edbaae4903230c26c009fef63ed7
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

V hello **cloudové služby (klasické)** oblasti hello Azure portálu, můžete aktualizovat roli služby nebo nasazení, zvýšení úrovně tooproduction dvoufázového nasazení, propojte prostředky tooyour cloudové služby, můžete zobrazit hello prostředky závislosti a škálování hello prostředky společně a odstraňte cloudovou službu nebo nasazení.

Další informace o tom, jak tooscale Cloudová služba je k dispozici [zde](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Postupy: aktualizovat roli služby cloudu nebo nasazení
Pokud potřebujete kód aplikace hello tooupdate pro cloudové služby, použijte **aktualizace** v okně služby hello cloudu. Můžete aktualizovat roli jednoho nebo všech rolí. tooupdate, můžete nahrát nový balíček služby nebo konfigurační soubor služby.

1. V hello [portál Azure][Azure portal], vyberte chcete tooupdate hello cloudovou službu. Tento krok otevře se okno instance služby cloud hello.
2. V okně hello, klikněte na tlačítko hello **aktualizace** tlačítko.

    ![Tlačítko Aktualizovat](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Hello nasazení aktualizace pomocí nového souboru balíku služeb (.cspkg) a konfigurační soubor služby (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Volitelně** aktualizovat označení nasazení hello a účet úložiště hello.
5. Pokud žádné role mít pouze jednu instanci role, vyberte hello **nasadit, i když jedna nebo víc rolí obsahuje jednu instanci** upgradu tooproceed tooenable hello.

    Azure můžete pouze zaručit 99,95 % dostupnost služby při aktualizaci služby cloud pokud každá role má aspoň dvě instance role (virtuální počítače). Se dvěma instancemi role jeden virtuální počítač zpracovává požadavky na klienta během hello jiných je aktualizována.

6. Zkontrolujte **spuštění nasazení** aktualizace hello toohave použijí po dokončení nahrávání hello hello balíčku.
7. Klikněte na tlačítko **OK** toobegin aktualizaci služby hello.

## <a name="how-to-swap-deployments-toopromote-a-staged-deployment-tooproduction"></a>Postupy: Prohodit nasazení toopromote tooproduction dvoufázového nasazení
Při rozhodování toodeploy novou verzi cloudové služby, fáze a otestovat novou verzi v prostředí s pracovní cloudové služby. Použití **Prohodit** tooswitch hello adresy URL, které hello dvě nasazení se podrobněji a zvýšení úrovně nového tooproduction verze.

Můžete zaměnit nasazení z hello **cloudové služby** stránky nebo hello řídicího panelu.

1. V hello [portál Azure][Azure portal], vyberte chcete tooupdate hello cloudovou službu. Tento krok otevře se okno instance služby cloud hello.
2. V okně hello, klikněte na tlačítko hello **Prohodit** tlačítko.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Hello následující potvrzovací výzvu otevře.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Po ověření, informace o nasazení hello, klikněte na tlačítko **OK** tooswap hello nasazení.

    swap nasazení Hello dochází rychle hello jediné, co se změní je hello virtuální IP adresy (VIP) pro nasazení hello.

    náklady na výpočetní toosave, můžete odstranit hello pracovní nasazení, jakmile ověříte, že produkčního nasazení funguje podle očekávání.

### <a name="common-questions-about-swapping-deployments"></a>Časté otázky týkající se nasazení vzájemná záměna

**Jaké jsou požadavky hello pro odkládací nasazení?**

Existují dva klíče požadavky pro úspěšné nasazení prohození:

- Pokud chcete pro produkční slot toouse statickou IP adresu, musíte rezervovat jeden pro vaše přípravný slot. Hello odkládacího souboru, jinak selže.

- Všechny instance role musí být spuštěna, než bude možné provést hello swap. Můžete zkontrolovat stav hello vašimi instancemi v okně Přehled hello hello portálu Azure. Alternativně můžete použít hello [Get-AzureRole](/powershell/module/azure/get-azurerole?view=azuresmps-3.7.0) příkazu v prostředí Windows PowerShell.

Všimněte si, že hostovaného operačního systému aktualizace a opravy operace služby může také způsobit nasazení prohození toofail. Další informace najdete v tématu [Poradce při potížích se nasazení služby cloud](cloud-services-troubleshoot-deployment-problems.md).

**Nesnižuje prohození výpadek své aplikaci? Jak by měly zpracovávat ho?**

Jak je popsáno v poslední části hello, prohození nasazení je obvykle rychlé, protože je právě změnu konfigurace pro vyrovnávání zatížení Azure hello. V některých případech ale ho můžete trvat deset nebo víc sekund a způsobit selhání přechodný připojení. toolimit dopad tooyour zákazníků, zvažte implementaci [logika opakovaných pokusů klienta](../best-practices-retry-general.md).

## <a name="how-to-link-a-resource-tooa-cloud-service"></a>Postupy: odkaz prostředků tooa cloudové služby
Hello portál Azure neobsahuje odkazy na prostředky společně jako hello aktuální portál Azure classic. Místo toho nasaďte další prostředky toohello stejnou skupinu prostředků, které jsou používány hello cloudové služby.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Postupy: odstranění nasazení a cloudové služby
Před odstraněním cloudové služby, je nutné odstranit každé existující nasazení.

náklady na výpočetní toosave, můžete odstranit hello pracovní nasazení, jakmile ověříte, že produkčního nasazení funguje podle očekávání. Fakturuje se výpočetní náklady pro instance nasazené rolí, které jsou zastaveny.

Použijte následující postup toodelete hello nasazení nebo cloudové služby.

1. V hello [portál Azure][Azure portal], vyberte chcete toodelete hello cloudovou službu. Tento krok otevře se okno instance služby cloud hello.
2. V okně hello, klikněte na tlačítko hello **odstranit** tlačítko.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Hello celý cloudové služby můžete odstranit kontrolou **Cloudová služba a její nasazení** nebo zvolte buď hello **produkční nasazení** nebo hello **pracovní nasazení**.

    ![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/delete-blade.png)

4. Klikněte na tlačítko hello **odstranit** tlačítko dole v hello.
5. toodelete hello Cloudová služba, klikněte na tlačítko **odstranění Cloudová služba**. Klikněte na výzvy k potvrzení hello **Ano**.

> [!NOTE]
> Pokud Cloudová služba je Odstraněná a podrobné monitorování je nastavena, musíte odstranit hello data ručně z vašeho účtu úložiště. Informace o kde toofind hello metriky tabulky najdete v tématu [to](cloud-services-how-to-monitor.md) článku.


## <a name="how-to-find-more-information-about-failed-deployments"></a>Postupy: Další informace o selhání nasazení
Hello **přehled** okno obsahuje stavového řádku v horní části hello. Po kliknutí na tlačítko panelu hello, nové okno se zobrazí veškeré informace o chybě. Pokud nasazení hello neobsahuje žádné chyby, okno informace hello je prázdný.

![Cloudové služby Swap](./media/cloud-services-how-to-manage-portal/status-info.png)



[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Další kroky
* [Obecná konfigurace cloudové služby](cloud-services-how-to-configure-portal.md).
* Zjistěte, jak příliš[nasazení cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).
