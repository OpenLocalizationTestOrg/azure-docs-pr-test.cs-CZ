---
title: "Připojení nástroje Configuration Manager k analýze protokolů | Microsoft Docs"
description: "Tento článek popisuje kroky pro připojení k analýze protokolů nástroje Configuration Manager a začněte analyzovat data."
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
ms.openlocfilehash: 62d31ed486458245156f7fc832294d662c62991e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-configuration-manager-to-log-analytics"></a>Připojení k analýze protokolů nástroje Configuration Manager
System Center Configuration Manager můžete připojit k analýze protokolů v OMS pro synchronizaci dat kolekce zařízení. Díky data z hierarchie nástroje Configuration Manager k dispozici v OMS.

## <a name="prerequisites"></a>Požadavky

Analýzy protokolů podporuje aktuální větve System Center Configuration Manager verze 1606 a vyšší.  

## <a name="configuration-overview"></a>Přehled konfigurace
Následující kroky shrnuje postup připojení k analýze protokolů nástroje Configuration Manager.  

1. V portálu pro správu Azure zaregistrujte nástroje Configuration Manager jako webovou aplikaci nebo webové rozhraní API app a ujistěte se, že máte ID klienta a tajný klíč klienta z registraci ze služby Azure Active Directory. V tématu [použití portálu k vytvoření služby Active Directory objekt zabezpečení aplikací a služeb, který mají přístup k prostředkům](../azure-resource-manager/resource-group-create-service-principal-portal.md) podrobné informace o provedení tohoto kroku.
2. Na portálu správy Azure [nástroje Configuration Manager (registrovaný webové aplikace) poskytnout oprávnění k přístupu k OMS](#provide-configuration-manager-with-permissions-to-oms).
3. V nástroji Configuration Manager [přidat připojení pomocí Průvodce přidáním připojení OMS](#add-an-oms-connection-to-configuration-manager).
4. V nástroji Configuration Manager [aktualizujte vlastnosti připojení](#update-oms-connection-properties) Pokud tajný klíč heslo nebo klienta, kdy vyprší platnost nebo dojde ke ztrátě.
5. Informace z portálu OMS [stáhněte a nainstalujte agenta Microsoft Monitoring Agent](#download-and-install-the-agent) v počítači se službou roli systému bodu lokality nástroje Configuration Manager service připojení. Agent odesílá data nástroje Configuration Manager k OMS.
6. V analýzy protokolů [importovat kolekce z nástroje Configuration Manager](#import-collections) jako skupiny počítačů.
7. V analýzy protokolů zobrazení dat z nástroje Configuration Manager jako [skupiny počítačů](log-analytics-computer-groups.md).

Další informace o připojení nástroje Configuration Manager k OMS na [synchronizovat data z nástroje Configuration Manager do služby Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx).

## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Poskytnout nástroje Configuration Manager oprávnění k OMS
Následující postup poskytuje Azure Management Portal s oprávněními pro přístup k OMS. Konkrétně musí udělit *role Přispěvatel* uživatelům ve skupině prostředků, aby bylo možné povolit portálu pro správu Azure Configuration Manager připojit k OMS.

> [!NOTE]
> Musíte zadat oprávnění v OMS pro nástroj Configuration Manager. Chybová zpráva jinak, obdržíte při použití Průvodce konfigurací služby v nástroji Configuration Manager.
>
>

1. Otevřete [portál Azure](https://portal.azure.com/) a klikněte na tlačítko **Procházet** > **analýzy protokolů (OMS)** otevřete okno analýzy protokolů (OMS).  
2. Na **analýzy protokolů (OMS)** okně klikněte na tlačítko **přidat** otevřete **pracovním prostorem OMS** okno.  
   ![Okno OMS](./media/log-analytics-sccm/sccm-azure01.png)
3. Na **pracovním prostorem OMS** okno, zadejte následující informace a potom klikněte na **OK**.

   * **Pracovní prostor OMS**
   * **Předplatné**
   * **Skupina prostředků**
   * **Umístění**
   * **Cenová úroveň**  
     ![Okno OMS](./media/log-analytics-sccm/sccm-azure02.png)  

     > [!NOTE]
     > Tento příklad vytvoří novou skupinu prostředků. Skupina prostředků slouží pouze k poskytování nástroje Configuration Manager s oprávněními k pracovním prostorem OMS v tomto příkladu.
     >
     >
4. Klikněte na tlačítko **Procházet** > **skupiny prostředků** otevřete **skupiny prostředků** okno.
5. V **skupiny prostředků** okno, klikněte na prostředek skupinu, kterou jste vytvořili výše, otevřete &lt;název skupiny prostředků&gt; okno nastavení.  
   ![okno nastavení skupiny prostředků](./media/log-analytics-sccm/sccm-azure03.png)
6. V &lt;název skupiny prostředků&gt; okno nastavení, klikněte na řízení přístupu (IAM) Chcete-li otevřít &lt;název skupiny prostředků&gt; oknem uživatelé.  
   ![okno Uživatelé skupiny prostředků](./media/log-analytics-sccm/sccm-azure04.png)  
7. V &lt;název skupiny prostředků&gt; oknem uživatelé, klikněte na tlačítko **přidat** otevřete **přidat přístup** okno.
8. V **přidat přístup** okně klikněte na tlačítko **vyberte roli**a pak vyberte **Přispěvatel** role.  
   ![Vybrat roli](./media/log-analytics-sccm/sccm-azure05.png)  
9. Klikněte na tlačítko **přidat uživatele**, vyberte uživatele nástroje Configuration Manager, klikněte na **vyberte**a potom klikněte na **OK**.  
   ![Přidání uživatelů](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Přidat připojení k OMS nástroje Configuration Manager
Chcete-li přidat připojení k OMS, musí mít prostředí nástroje Configuration Manager [spojovací bod služby](https://technet.microsoft.com/library/mt627781.aspx) konfigurován pro online režim.

1. V **správy** prostoru nástroje Configuration Manager, vyberte **OMS konektor**. Tím se otevře **Průvodce přidáním připojení OMS**. Vyberte **Další**.
2. Na **Obecné** obrazovky, potvrďte, že jste dokončili následující akce a mít podrobnosti pro každou položku a pak vyberte **Další**.

   1. Na portálu Azure Management Portal jste registrováni nástroje Configuration Manager jako webovou aplikaci nebo webové rozhraní API aplikaci a že máte [ID klienta z registrace](../active-directory/active-directory-integrating-applications.md).
   2. Na portálu správy Azure jste vytvořili tajný klíč aplikace pro aplikaci registrovanou v Azure Active Directory.  
   3. V portálu pro správu Azure jste zadali registrované webové aplikace s oprávněním pro přístup k OMS.  
      ![Připojení k stránka průvodce Obecné OMS](./media/log-analytics-sccm/sccm-console-general01.png)
3. Na **Azure Active Directory** obrazovky, konfigurace nastavení připojení k OMS tím, že poskytuje vaší **klienta** , **ID klienta** , a **tajný klíč klienta ** , pak vyberte **Další**.  
   ![Připojení k stránce OMS Průvodce Azure Active Directory](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Pokud můžete provést všechny postupy úspěšně, pak informace na **konfigurace připojení OMS** obrazovky se automaticky zobrazí na této stránce. Informace o nastavení připojení, které by se měla objevit pro vaše **předplatného Azure** , **skupina prostředků Azure** , a **pracovní prostor služby Operations Management Suite**.  
   ![Připojení k stránce OMS Průvodce OMS připojení](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. Průvodce se připojí ke službě OMS pomocí informace, které jste vstup. Vyberte kolekce zařízení, které chcete synchronizovat s OMS a pak klikněte na tlačítko **přidat**.  
   ![Vyberte kolekce](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Ověřte nastavení připojení v **Souhrn** obrazovky a pak vyberte **Další**. **Průběh** obrazovky ukazuje stav připojení a pak by měl **Complete**.

> [!NOTE]
> OMS musí připojit k lokalitě nejvyšší úrovně ve vaší hierarchii. Pokud připojení OMS na samostatnou primární lokalitou a poté přidejte lokalitu centrální správy pro vaše prostředí, budete muset odstranit a znovu vytvořte připojení OMS v nové hierarchii.
>
>

Po propojení nástroje Configuration Manager na OMS, můžete přidat nebo odebrat kolekce a zobrazit vlastnosti připojení OMS.

## <a name="update-oms-connection-properties"></a>Aktualizovat vlastnosti připojení OMS
Pokud heslo nebo klienta tajný klíč někdy vyprší platnost nebo dojde ke ztrátě, budete muset ručně aktualizovat vlastnosti připojení OMS.

1. V nástroji Configuration Manager přejděte na **cloudové služby** , pak vyberte **OMS konektor** otevřete **vlastnosti připojení OMS** stránky.
2. Na této stránce, klikněte na tlačítko **Azure Active Directory** zobrazíte vaše **klienta**, **ID klienta**, **klienta tajný klíč vypršení platnosti**. **Ověřte** vaše **tajný klíč klienta** Pokud vypršela platnost.

## <a name="download-and-install-the-agent"></a>Stáhněte a nainstalujte agenta
1. Na portálu OMS [stáhnout instalační soubor agenta z OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. K instalaci a konfiguraci agenta v počítači se systémem roli systému bodu lokality nástroje Configuration Manager service připojení, použijte jednu z následujících metod:
   * [Instalace agenta pomocí instalačního programu](log-analytics-windows-agents.md#install-the-agent-using-setup)
   * [Instalace agenta pomocí příkazového řádku](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
   * [Instalace agenta pomocí DSC v Azure Automation.](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)

## <a name="import-collections"></a>Importovat kolekce
Po připojení k OMS přidán do nástroje Configuration Manager a nainstalovali agenta na počítači se systémem připojení nástroje Configuration Manager služby role systému lokality bodu, dalším krokem je k importování kolekcí z nástroje Configuration Manager v OMS jako počítač skupiny.

Po povolení import informace o členství v kolekci je načíst každé 3 hodiny zachovat aktuální členství kolekce. Můžete zakázat import kdykoli.

1. Na portálu OMS, klikněte na tlačítko **nastavení**.
2. Klikněte **skupiny počítačů** a pak klikněte **SCCM** kartě.
3. Vyberte **členství v kolekcích Import Configuration Manager** a pak klikněte na **Uložit**.  
   ![Skupiny počítačů - karta SCCM](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Zobrazení dat z nástroje Configuration Manager
Po připojení k OMS přidán do nástroje Configuration Manager a nainstalovali agenta na počítači se systémem roli systému bodu lokality nástroje Configuration Manager service připojení, data od agenta se odešlou do OMS. Vaše kolekce nástroje Configuration Manager v OMS, se zobrazí jako [skupiny počítačů](log-analytics-computer-groups.md). Můžete zobrazit skupiny z **nástroje Configuration Manager** v části **skupiny počítačů** v **nastavení**.

Po importu kolekce, uvidíte, kolik počítačů s členství v kolekcích byly zjištěny. Rovněž uvidíte počet kolekcí, které byly naimportovány.

![Skupiny počítačů - karta SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Po kliknutí na tlačítko buď jednu, otevře se hledání zobrazí všechny skupiny pro importované nebo všechny počítače, které patří ke každé skupině. Pomocí [hledání protokolů](log-analytics-log-searches.md), můžete spustit podrobné analýzy dat nástroje Configuration Manager.

## <a name="next-steps"></a>Další kroky
* Použití [hledání protokolů](log-analytics-log-searches.md) Chcete-li zobrazit podrobné informace o data nástroje Configuration Manager.
