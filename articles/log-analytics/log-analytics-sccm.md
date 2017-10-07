---
title: "aaaConnect tooLog nástroje Configuration Manager Analytics | Microsoft Docs"
description: "Tento článek popisuje kroky tooconnect hello tooLog nástroje Configuration Manager analýzy a analýza dat spustit."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f2298bd7-18d7-4371-b24a-7f9f15f06d66
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: banders
ms.openlocfilehash: dc50ebc46020a806d99d1a3e3d0e91fd09ad2c32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-configuration-manager-toolog-analytics"></a>Připojení nástroje Configuration Manager tooLog Analytics
System Center Configuration Manager tooLog analýzy v OMS toosync zařízení shromažďování dat se můžete připojit. Díky data z hierarchie nástroje Configuration Manager k dispozici v OMS.

## <a name="prerequisites"></a>Požadavky

Analýzy protokolů podporuje aktuální větve System Center Configuration Manager verze 1606 a vyšší.  

## <a name="configuration-overview"></a>Přehled konfigurace
Následující kroky Hello shrnuje hello proces tooconnect Analytics tooLog nástroje Configuration Manager.  

1. V hello portálu pro správu Azure zaregistrujte se jako webovou aplikaci nebo webové rozhraní API aplikace Configuration Manager a ujistěte se, že máte hello ID a klienta tajný klíč klienta z hello registrace z Azure Active Directory. V tématu [pomocí aplikace portálu toocreate Active Directory a objektu služby, které mají přístup k prostředkům](../azure-resource-manager/resource-group-create-service-principal-portal.md) podrobné informace o provedení tohoto kroku.
2. V portálu pro správu Azure, hello [nástroje Configuration Manager (hello registrované webové aplikace) poskytnout oprávnění tooaccess OMS](#provide-configuration-manager-with-permissions-to-oms).
3. V nástroji Configuration Manager [přidat připojení pomocí Průvodce přidáním OMS připojení hello](#add-an-oms-connection-to-configuration-manager).
4. V nástroji Configuration Manager [aktualizujte vlastnosti připojení hello](#update-oms-connection-properties) Pokud hello heslo nebo klienta tajný klíč, kdy vyprší platnost nebo dojde ke ztrátě.
5. Informace z portálu OMS hello [stáhněte a nainstalujte agenta Microsoft Monitoring Agent hello](#download-and-install-the-agent) role systému lokality bodu připojení služby nástroje Configuration Manager hello hello počítače. Hello agent odesílá data tooOMS nástroje Configuration Manager.
6. V analýzy protokolů [importovat kolekce z nástroje Configuration Manager](#import-collections) jako skupiny počítačů.
7. V analýzy protokolů zobrazení dat z nástroje Configuration Manager jako [skupiny počítačů](log-analytics-computer-groups.md).

Další informace o připojení nástroje Configuration Manager tooOMS v [synchronizaci dat z nástroje Configuration Manager toohello Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-toooms"></a>Poskytnout oprávnění tooOMS nástroje Configuration Manager
Hello následující postup popisuje hello Azure Management Portal s oprávněními tooaccess OMS. Konkrétně musí udělit hello *role Přispěvatel* toousers ve skupině prostředků hello v pořadí tooallow hello portálu pro správu Azure tooconnect tooOMS nástroje Configuration Manager.

> [!NOTE]
> Musíte zadat oprávnění v OMS pro nástroj Configuration Manager. Chybová zpráva jinak, obdržíte při použití Průvodce konfigurací hello v nástroji Configuration Manager.
>
>

1. Otevřete hello [portál Azure](https://portal.azure.com/) a klikněte na tlačítko **Procházet** > **analýzy protokolů (OMS)** tooopen hello analýzy protokolů (OMS) okno.  
2. Na hello **analýzy protokolů (OMS)** okně klikněte na tlačítko **přidat** tooopen hello **pracovním prostorem OMS** okno.  
   ![Okno OMS](./media/log-analytics-sccm/sccm-azure01.png)
3. Na hello **pracovním prostorem OMS** okno, zadejte hello následující informace a pak klikněte na tlačítko **OK**.

   * **Pracovní prostor OMS**
   * **Předplatné**
   * **Skupina prostředků**
   * **Umístění**
   * **Cenová úroveň**  
     ![Okno OMS](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > Hello příklad vytvoří novou skupinu prostředků. Skupina prostředků Hello je pouze použité tooprovide nástroje Configuration Manager s pracovním prostorem OMS toohello oprávnění v tomto příkladu.
     >
     >
4. Klikněte na tlačítko **Procházet** > **skupiny prostředků** tooopen hello **skupiny prostředků** okno.
5. V hello **skupiny prostředků** okně klikněte na tlačítko hello skupinu prostředků, kterou jste vytvořili výše tooopen hello &lt;název skupiny prostředků&gt; okno nastavení.  
   ![okno nastavení skupiny prostředků](./media/log-analytics-sccm/sccm-azure03.png)
6. V hello &lt;název skupiny prostředků&gt; okno nastavení, klikněte na tlačítko přístup ovládacího prvku (IAM) tooopen hello &lt;název skupiny prostředků&gt; oknem uživatelé.  
   ![okno Uživatelé skupiny prostředků](./media/log-analytics-sccm/sccm-azure04.png)  
7. V hello &lt;název skupiny prostředků&gt; oknem uživatelé, klikněte na tlačítko **přidat** tooopen hello **přidat přístup** okno.
8. V hello **přidat přístup** okně klikněte na tlačítko **vyberte roli**a potom vyberte hello **Přispěvatel** role.  
   ![Vybrat roli](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klikněte na tlačítko **přidat uživatele**, vyberte uživatele hello nástroje Configuration Manager, klikněte na **vyberte**a potom klikněte na **OK**.  
   ![Přidání uživatelů](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-tooconfiguration-manager"></a>Přidat OMS připojení tooConfiguration Manager
V pořadí tooadd připojení k OMS, musí mít prostředí nástroje Configuration Manager [spojovací bod služby](https://technet.microsoft.com/library/mt627781.aspx) konfigurován pro online režim.

1. V hello **správy** prostoru nástroje Configuration Manager, vyberte **OMS konektor**. Tím se otevře hello **Průvodce přidáním připojení OMS**. Vyberte **Další**.
2. Na hello **Obecné** obrazovky, potvrďte, že jste dokončili hello následující akce a mít podrobnosti pro každou položku a pak vyberte **Další**.

   1. V hello portálu pro správu Azure, jako webové aplikace nebo webové rozhraní API aplikaci jste registrováni nástroje Configuration Manager a zda mají hello [ID klienta z hello registrace](../active-directory/active-directory-integrating-applications.md).
   2. V hello portálu pro správu Azure jste vytvořili tajný klíč aplikace pro hello registrované aplikaci v Azure Active Directory.  
   3. V hello portálu pro správu Azure jste zadali hello registrované webové aplikace s oprávnění tooaccess OMS.  
      ![Stránka průvodce Obecné tooOMS připojení](./media/log-analytics-sccm/sccm-console-general01.png)
3. Na hello **Azure Active Directory** obrazovky, nakonfigurujte nastavení tooOMS vaše připojení tím, že poskytuje vaší **klienta** , **ID klienta** , a **tajný klíč klienta**  , pak vyberte **Další**.  
   ![Stránka průvodce Azure Active Directory tooOMS připojení](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Pokud jste provést všechny hello další postupy úspěšně, pak hello informace o hello **konfigurace připojení OMS** obrazovky se automaticky zobrazí na této stránce. Informace o nastavení připojení hello by se měla objevit pro vaše **předplatného Azure** , **skupina prostředků Azure** , a **pracovní prostor služby Operations Management Suite**.  
   ![Stránka průvodce OMS připojení tooOMS připojení](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. Hello průvodce se připojí toohello OMS služby pomocí hello informace, které jste vstup. Vyberte kolekce zařízení hello má toosync s OMS a pak klikněte na **přidat**.  
   ![Vyberte kolekce](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Ověřte nastavení připojení na hello **Souhrn** obrazovky a pak vyberte **Další**. Hello **průběh** obrazovky ukazuje stav připojení hello, pak by měl **Complete**.

> [!NOTE]
> Je nutné připojit OMS toohello nejvyšší úrovně lokalitu ve vaší hierarchii. Pokud připojení OMS tooa samostatnou primární lokalitou a poté přidejte prostředí tooyour lokality centrální správy, budete mít toodelete a znovu vytvořte připojení OMS hello v nové hierarchii hello.
>
>

Po propojení tooOMS nástroje Configuration Manager, můžete přidat nebo odebrat kolekce a zobrazit vlastnosti hello hello OMS připojení.

## <a name="update-oms-connection-properties"></a>Aktualizovat vlastnosti připojení OMS
Pokud heslo nebo klienta tajný klíč někdy vyprší platnost nebo dojde ke ztrátě, budete potřebovat vlastnosti připojení toomanually aktualizace hello OMS.

1. V nástroji Configuration Manager přejděte příliš**cloudové služby** , pak vyberte **OMS konektor** tooopen hello **vlastnosti připojení OMS** stránky.
2. Na této stránce, klikněte na tlačítko hello **Azure Active Directory** kartě tooview vaše **klienta**, **ID klienta**, **klienta tajný klíč vypršení platnosti**. **Ověřte** vaše **tajný klíč klienta** Pokud vypršela platnost.

## <a name="download-and-install-hello-agent"></a>Stáhněte a nainstalujte agenta hello
1. Na portálu OMS hello [stažení hello agenta instalační soubor od OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Použijte jednu z následujících metod tooinstall hello a konfiguraci agenta hello hello počítače se systémem služby nástroje Configuration Manager hello, připojení role systému lokality bodu:
   * [Nainstalujte agenta hello pomocí instalačního programu](log-analytics-windows-agents.md#install-the-agent-using-setup)
   * [Nainstalujte agenta hello hello příkazového řádku](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
   * [Instalace agenta hello pomocí DSC v Azure Automation.](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)

## <a name="import-collections"></a>Importovat kolekce
Po přidání tooConfiguration připojení OMS Manager a nainstalovat agenta hello role systému lokality bodu připojení služby nástroje Configuration Manager hello hello počítače, hello dalším krokem je kolekce tooimport z nástroje Configuration Manager v OMS jako skupiny počítačů.

Po povolení import je načíst informace o členství kolekce hello každých 3 hodiny tookeep hello členství v kolekcích aktuální. Můžete zvolit import toodisable kdykoli.

1. Na portálu OMS hello, klikněte na tlačítko **nastavení**.
2. Klikněte na tlačítko hello **skupiny počítačů** a pak klikněte hello **SCCM** kartě.
3. Vyberte **členství v kolekcích Import Configuration Manager** a pak klikněte na **Uložit**.  
   ![Skupiny počítačů - karta SCCM](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Zobrazení dat z nástroje Configuration Manager
Poté, co jste přidali OMS připojení tooConfiguration správce a nainstalovat agenta hello hello počítače systému role lokality spojovacího bodu hello nástroje Configuration Manager service, data z agenta hello odeslána tooOMS. Vaše kolekce nástroje Configuration Manager v OMS, se zobrazí jako [skupiny počítačů](log-analytics-computer-groups.md). Můžete zobrazit skupiny hello z hello **nástroje Configuration Manager** v části **skupiny počítačů** v **nastavení**.

Po hello, které jsou importovány kolekce se zobrazí, kolik počítačů s členství v kolekcích byly zjištěny. Zobrazí se také hello počet kolekcí, které byly naimportovány.

![Skupiny počítačů - karta SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Po kliknutí na tlačítko buď jeden otevře vyhledávání, zobrazování buď všechny hello importovat skupiny nebo všechny počítače, které patří tooeach skupiny. Pomocí [hledání protokolů](log-analytics-log-searches.md), můžete spustit podrobné analýzy dat nástroje Configuration Manager.

## <a name="next-steps"></a>Další kroky
* Použití [hledání protokolů](log-analytics-log-searches.md) tooview podrobné informace o data nástroje Configuration Manager.
