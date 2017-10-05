---
title: "Připojení ITSM v OMS IT Service Management Connector | Microsoft Docs"
description: "Připojení ITSM produkty nebo služby s konektorem správy IT služby v OMS centrálně monitorovat a spravovat ITSM pracovní položky."
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: v-jysur
ms.openlocfilehash: e4f2e0a23aa52a0e02e7047916b77fb15107defa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector-preview"></a>Připojení ITSM produkty nebo služby s IT Service Management Connector (Preview)
Tento článek obsahuje informace o tom, jak připojit ITSM produktu nebo služby pro správu konektoru služby IT v OMS a centrálně spravovat pracovní položky. Další informace o konektoru služby správy IT, naleznete v [přehled](log-analytics-itsmc-overview.md).

Jsou podporovány tyto produkty nebo služby:

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-oms)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-oms)
- [Provance](#connect-provance-to-it-service-management-connector-in-oms)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-oms"></a>Připojení produktu System Center Service Manager k IT službu konektoru Management v OMS

Následující části obsahují podrobnosti o tom, jak připojit svůj produkt System Center Service Manager ke konektoru služby správy IT v OMS.

### <a name="prerequisites"></a>Požadavky

Zajistěte, že abyste měli splnit následující požadavky:

- IT služby správy nainstalovaný konektor.
Další informace: [konfigurace](log-analytics-itsmc-overview.md#configuration).
- Aplikace webového portálu Service Manager (webové aplikace) nasazená a nakonfigurovaná. Informace o webové aplikace je [zde](#create-and-deploy-service-manager-web-app-service).
- Hybridní připojení vytvořený a nakonfigurovaný. Další informace: [konfigurace hybridní připojení](#configure-the-hybrid-connection).
- Podporované verze portálu Service Manager: 2012 R2 nebo 2016.
- Role uživatele: [operátor s rozšířenými oprávněními](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Postup připojení

Pro připojení instanci aplikace System Center Service Manager do konektoru správy IT služby použijte následující postup:

1. Přejděte na **OMS** >**nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.

    ![Portál Service manager ](./media/log-analytics-itsmc/itsmc-service-manager-connection.png)
3. Zadejte informace, jak je popsáno v následující tabulce a klikněte na tlačítko **Uložit** k vytvoření připojení:

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název pro instanci System Center Service Manager, kterou chcete připojit pomocí konektoru služby správy IT.  Můžete použít tento název později při zobrazení analýzy podrobné protokolů nebo nakonfigurujte pracovních položek v této instanci. |
| **Vyberte typ připojení**   | Vyberte **System Center Service Manager**. |
| **Adresa URL serveru**   | Zadejte adresu URL portálu Service Manager webové aplikace. Další informace o portálu Service Manager webové aplikace je [zde](#create-and-deploy-service-manager-web-app-service).
| **ID klienta**   | Zadejte ID klienta, který jste vygenerovali (pomocí automatické skript) pro ověřování webové aplikace. Další informace o automatizované skript je [sem.](log-analytics-itsmc-service-manager-script.md)|
| **Tajný klíč klienta**   | Zadejte sdílený tajný klíč klienta vygenerované číslem ID této.   |
| **Obor synchronizace dat**   | Vyberte pracovních položek portálu Service Manager, které chcete synchronizovat prostřednictvím konektoru služby správy IT.  Tyto pracovní položky budou importovány do analýzy protokolů. **Možnosti:** incidenty, žádosti o změnu.|
| **Synchronizaci dat** | Zadejte počet uplynulých dní, které chcete data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete vytvořit položky konfigurace v rámci ITSM produktu. Při výběru OMS vytvoří ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v podporovaných ITSM systému. **Výchozí**: zakázáno. |

Když se úspěšně připojil a synchronizovat:

- Vybrané pracovní položky z portálu Service Manager jsou importovány do OMS **analýzy protokolů.** Můžete zobrazit souhrn těchto pracovní položky na dlaždici konektoru služby správy IT.

- Od OMS můžete vytvořit incidenty z výstrah OMS nebo z protokolu hledání v této instanci portálu Service Manager.

Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Vytvoření a nasazení portálu Service Manager webové aplikace služby

Připojit místní Service Manager pomocí konektoru služby správy IT na OMS, společnost Microsoft vytvořila portálu Service Manager webovou aplikaci Githubu.

Pokud chcete nastavit ITSM webové aplikace pro portálu Service Manager, postupujte takto:

- **Nasazení webové aplikace** – nasazení webové aplikace, nastavte vlastnosti a ověřování s Azure AD. Můžete nasadit webovou aplikaci pomocí [automatizované skriptu](log-analytics-itsmc-service-manager-script.md) , společnost Microsoft poskytuje můžete.
- **Konfigurace hybridní připojení** - [konfigurovat toto připojení](#configure-the-hybrid-connection), je nutné ručně.

#### <a name="deploy-the-web-app"></a>Nasazení webové aplikace
Použít automatizované [skriptu](log-analytics-itsmc-service-manager-script.md) nasazení webové aplikace, nastavte vlastnosti a ověřování s Azure AD.

Spusťte skript tím, že poskytuje následující požadované podrobnosti:

- Podrobnosti o předplatném Azure
- Název skupiny prostředků
- Umístění
- Podrobnosti o serveru portálu Service Manager (název serveru, domény, uživatelské jméno a heslo)
- Předpona názvu lokality pro vaši webovou aplikaci
- Namespace sběrnice.

Tento skript vytvoří webovou aplikaci pomocí názvu, který jste zadali (spolu s několika další řetězce, aby byla zajištěna jedinečnost). Vygeneruje **adresa URL webové aplikace**, **ID klienta** a **tajný klíč klienta**.

Uložit hodnoty je používat při vytváření připojení ke konektoru služby správy IT.

**Kontrola instalace webové aplikace**

1. Přejděte na **portál Azure** > **prostředky**.
2. Vyberte webovou aplikaci, klikněte na **nastavení** > **nastavení aplikace**.
3. Zkontrolujte informace o instanci portálu Service Manager, který jste zadali v době nasazení aplikace pomocí skriptu.

### <a name="configure-the-hybrid-connection"></a>Konfigurace hybridní připojení

Použijte následující postup ke konfiguraci hybridního připojení, která se připojuje instanci portálu Service Manager pomocí konektoru služby správy IT v OMS.

1. Aplikaci webového portálu Service Manager, najdete v části **prostředky Azure**.
2. Klikněte na tlačítko **nastavení** > **sítě**.
3. V části **hybridní připojení**, klikněte na tlačítko **nakonfigurovat koncové body hybridního připojení**.

    ![Hybridní připojení sítě](./media/log-analytics-itsmc/itsmc-hybrid-connection-networking-and-end-points.png)
4. V **hybridní připojení** okně klikněte na tlačítko **přidat hybridní připojení**.

    ![Přidat hybridní připojení.](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-add.png)

5. V **přidat hybridní připojení** okně klikněte na tlačítko **vytvořit nové hybridní připojení**.

    ![Nové hybridní připojení](./media/log-analytics-itsmc/itsmc-create-new-hybrid-connection.png)

6. Zadejte následující hodnoty:

    - **Název koncového bodu**: Zadejte název nové hybridní připojení.
    -  **Koncový bod hostitele**: plně kvalifikovaný název domény serveru pro správu portálu Service Manager.
    - **Koncový Port**: Zadejte 5724
    - **Obor názvů sběrnice**: použití existujícího oboru názvů sběrnice nebo vytvořte novou.
    - **Umístění**: Vyberte umístění.
    -  **Název**: Zadejte název, který sběrnice při jeho vytváření.

    ![Hybridní připojení hodnoty](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-values.png)
6. Klikněte na tlačítko **OK** zavřete **vytvořit hybridní připojení** okno a spusťte vytvoření hybridního připojení.

    Po vytvoření hybridního připojení se zobrazí v okně.

7. Po vytvoření hybridního připojení vyberte připojení a klikněte na tlačítko **přidat vybrané hybridní připojení**.

    ![Nové hybridní připojení](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>Konfigurace nastavení naslouchacího procesu

Použijte následující postup ke konfiguraci nastavení naslouchacího procesu pro hybridní připojení.

1. V **hybridní připojení** okně klikněte na tlačítko **stáhnout správce připojení** a nainstalujte ji na počítači, kde je spuštěna instance System Center Service Manager.

    Po dokončení instalace **uživatelského rozhraní správce hybridního připojení** možnost je k dispozici v části **spustit** nabídky.

2. Klikněte na tlačítko **uživatelského rozhraní správce hybridního připojení** , zobrazí se výzva k zadání pověření Azure.

3. Přihlášení pomocí svých přihlašovacích údajů Azure a vybrat své předplatné, které bylo vytvořeno hybridní připojení.

4. Klikněte na **Uložit**.

Hybridní připojení se úspěšně připojil.

![úspěšné hybridní připojení.](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Po hybridním vytvoření připojení, ověřte a otestovat připojení návštěvou nasazené aplikace webového portálu Service Manager. Zkontrolujte, zda že je připojení úspěšné, před pokusem o připojení ke konektoru služby správy IT v OMS.

Následující obrázek zobrazuje podrobnosti o úspěšném připojení:

![Test hybridní připojení](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-oms"></a>Připojení ServiceNow k IT službu konektoru Management v OMS

Následující části obsahují podrobnosti o tom, jak připojit svůj produkt ServiceNow ke konektoru služby správy IT v OMS.

### <a name="prerequisites"></a>Požadavky

Zajistěte, že abyste měli splnit následující požadavky:

- IT služby správy nainstalovaný konektor. Další informace: [konfigurace.](log-analytics-itsmc-overview.md#configuration)
- ServiceNow podporována verze – Fuji, Genevy, Helsinkách.

Správci ServiceNow musíte provést následující v jejich instance ServiceNow:
- Generování ID klienta a tajný klíč klienta pro produkt ServiceNow. Informace o tom, jak vygenerovat ID klienta a tajný klíč najdete v tématu [instalace OAuth](http://wiki.servicenow.com/index.php?title=OAuth_Setup).
- Nainstalujte aplikaci uživatele pro integraci Microsoft OMS (ServiceNow aplikace). [Další informace](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0 ).
- Umožňuje vytvořte roli uživatele integrace pro nainstalovanou aplikaci uživatele. Informace o tom, jak vytvořit roli uživatele integrace [zde](#create-integration-user-role-in-servicenow-app).


### <a name="connection-procedure"></a>**Postup připojení**

Pomocí následujícího postupu vytvořte připojení ServiceNow:

1. Přejděte na **OMS** > **nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.

    ![Připojení ServiceNow](./media/log-analytics-itsmc/itsmc-servicenow-connection.png)

3. Zadejte informace, jak je popsáno v následující tabulce a klikněte na tlačítko **Uložit** k vytvoření připojení:

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název instance ServiceNow, který chcete připojit pomocí konektoru služby správy IT.  Tento název použijete později v OMS při zobrazení analýzy podrobné protokolů nebo na tento ITSM nakonfigurujte pracovních položek. |
| **Vyberte typ připojení**   | Vyberte **ServiceNow**. |
| **Uživatelské jméno**   | Zadejte uživatelské jméno integrace, které jste vytvořili v aplikaci ServiceNow pro podporu připojení ke konektoru služby správy IT. Další informace: [ServiceNow vytvořit roli uživatele aplikace](#create-integration-user-role-in-servicenow-app).|
| **Heslo**   | Zadejte heslo přidružené k této uživatelské jméno. **Poznámka:**: uživatelské jméno a heslo pro generování pouze tokeny ověřování se používají a nejsou uložit kdekoli v rámci služby OMS.  |
| **Adresa URL serveru**   | Zadejte adresu URL instance ServiceNow, který chcete připojit ke konektoru služby správy IT. |
| **ID klienta**   | Zadejte ID klienta, který chcete používat pro ověřování OAuth2, který jste vygenerovali dříve.  Další informace o generování ID klienta a tajný klíč: [instalace OAuth](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Tajný klíč klienta**   | Zadejte sdílený tajný klíč klienta vygenerované číslem ID této.   |
| **Obor synchronizace dat**   | Vyberte ServiceNow pracovní položky, které chcete synchronizovat do OMS prostřednictvím konektoru služby správy IT.  Vybrané hodnoty jsou importovány do analýzy protokolů.   **Možnosti:** incidenty a žádosti o změnu.|
| **Synchronizaci dat** | Zadejte počet uplynulých dní, které chcete data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete vytvořit položky konfigurace v rámci ITSM produktu. Při výběru OMS vytvoří ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v podporovaných ITSM systému. **Výchozí**: zakázáno. |


Když se úspěšně připojil a synchronizovat:

- Vybrat pracovní položky z připojení ServiceNow jsou importovány do OMS Log Analytics.  Můžete zobrazit souhrn těchto pracovní položky na dlaždici konektoru služby správy IT.
- Incidenty, výstrahy a událostí můžete vytvořit z OMS výstrahy nebo protokolu hledání u této instance ServiceNow.  


Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-integration-user-role-in-servicenow-app"></a>Umožňuje vytvořit roli uživatele integrace v aplikaci ServiceNow

Uživatel takto:

1.  Přejděte [ServiceNow úložiště](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0) a nainstalujte **uživatele aplikace pro integraci produktů Microsoft OMS a ServiceNow** do vaší Instance ServiceNow.
2.  Po instalaci, přejděte v levém navigačním panelu ServiceNow instance, vyhledávání a vyberte integrátor Microsoft OMS.  
3.  Klikněte na tlačítko **kontrolní seznam pro instalaci**.

    Stav se zobrazí jako **nedokončila** Pokud dosud vytvořit roli uživatele.

4.  Do textových polí vedle **uživatel integrace vytvořit**, zadejte uživatelské jméno pro uživatele, který můžete připojit ke konektoru služby správy IT v OMS.
5.  Zadejte heslo pro tohoto uživatele a klikněte na tlačítko **OK**.  

>[!NOTE]

> Tato pověření slouží k navázání připojení ServiceNow v OMS.

Zobrazí se výchozí role přiřazené se nově vytvořeného uživatele.

Výchozí role:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.User
-   ITIL
-   template_editor
-   view_changer

Jakmile uživatel je úspěšně vytvořen, stav **zkontrolujte kontrolní seznam instalace** přesune na dokončeno, výpis podrobnosti o roli uživatele pro aplikaci vytvořili.

> [!NOTE]

> Umožňuje uživateli vytvořit **výstrahy** a **události** v ServiceNow od OMS:

> - Zkontrolujte, zda že máte modul událostí správy nainstalovaná na vaší instance ServiceNow.

> - Přidejte následující role uživatele integrace:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-oms"></a>Připojit k službě, IT Provance konektoru Management v OMS

Následující části obsahují podrobnosti o tom, jak připojit svůj produkt Provance ke konektoru služby správy IT v OMS.

### <a name="prerequisites"></a>Požadavky

Zajistěte, že abyste měli splnit následující požadavky:

- IT služby správy nainstalovaný konektor. Další informace: [konfigurace](log-analytics-itsmc-overview.md#configuration).
- Provance aplikace by měla být zaregistrován u služby Azure AD - a ID klienta je k dispozici. Podrobné informace najdete v tématu [konfiguraci ověřování služby active directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
- Role uživatele: správce.

### <a name="connection-procedure"></a>Postup připojení

Pomocí následujícího postupu vytvoříte připojení typu Provance:

1. Přejděte na **OMS** > **nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.  

    ![Provance připojení](./media/log-analytics-itsmc/itsmc-provance-connection.png)
3. Zadejte informace, jak je popsáno v následující tabulce a klikněte na tlačítko **Uložit** k vytvoření připojení.

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název pro instanci Provance, kterou chcete připojit pomocí konektoru služby správy IT.  Tento název použijete později v OMS při zobrazení analýzy podrobné protokolů nebo na tento ITSM nakonfigurujte pracovních položek. |
| **Vyberte typ připojení**   | Vyberte **Provance**. |
| **Uživatelské jméno**   | Zadejte uživatelské jméno, které můžete připojit ke konektoru služby správy IT.    |
| **Heslo**   | Zadejte heslo přidružené k této uživatelské jméno. **Poznámka:** uživatelské jméno a heslo pro generování pouze tokeny ověřování se používají a nejsou uložit kdekoli v rámci služby OMS. _|
| **Adresa URL serveru**   | Zadejte adresu URL Provance instanci, kterou chcete připojit ke konektoru služby správy IT. |
| **ID klienta**   | Zadejte ID klienta pro toto připojení, který jste vygenerovali v instanci Provance ověřování.  Další informace o ID klienta, viz [konfiguraci ověřování služby active directory](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Obor synchronizace dat**   | Vyberte Provance pracovní položky, které chcete synchronizovat do OMS prostřednictvím konektoru služby správy IT.  Tyto pracovní položky budou importovány do analýzy protokolů.   **Možnosti:** incidenty, žádosti o změnu.|
| **Synchronizaci dat** | Zadejte počet uplynulých dní, které chcete data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete vytvořit položky konfigurace v rámci ITSM produktu. Při výběru OMS vytvoří ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v podporovaných ITSM systému. **Výchozí**: zakázáno.|

Když se úspěšně připojil a synchronizovat:

- Vybrané pracovní položky z Provance připojení jsou importovány do OMS **analýzy protokolů.**  Můžete zobrazit souhrn těchto pracovní položky na dlaždici konektoru služby správy IT.
- V této instanci Provance, můžete vytvořit incidenty a události z OMS výstrahy nebo hledání protokolů.

Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

## <a name="connect-cherwell-to-it-service-management-connector-in-oms"></a>Připojit k službě, IT Cherwell konektoru Management v OMS

Následující části obsahují podrobnosti o tom, jak připojit svůj produkt Cherwell ke konektoru služby správy IT v OMS.

### <a name="prerequisites"></a>Požadavky

Zajistěte, že abyste měli splnit následující požadavky:

- IT služby správy nainstalovaný konektor. Další informace: [konfigurace](log-analytics-itsmc-overview.md#configuration).
- Vygeneruje ID klienta. Další informace: [generování ID klienta pro Cherwell](#generate-client-id-for-cherwell).
- Role uživatele: správce.

### <a name="connection-procedure"></a>Postup připojení

Pomocí následujícího postupu vytvoříte připojení typu Cherwell:

1. Přejděte na **OMS** >  **nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.  

    ![Id uživatele Cherwell](./media/log-analytics-itsmc/itsmc-cherwell-connection.png)

3. Zadejte informace, jak je popsáno v následující tabulce a klikněte na tlačítko **Uložit** k vytvoření připojení.

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název pro instanci Cherwell, kterou chcete připojit ke konektoru služby správy IT.  Tento název použijete později v OMS při zobrazení analýzy podrobné protokolů nebo na tento ITSM nakonfigurujte pracovních položek. |
| **Vyberte typ připojení**   | Vyberte **Cherwell.** |
| **Uživatelské jméno**   | Zadejte uživatelské jméno Cherwell, zda se může připojit ke konektoru služby správy IT. |
| **Heslo**   | Zadejte heslo přidružené k této uživatelské jméno. **Poznámka:** uživatelské jméno a heslo pro generování pouze tokeny ověřování se používají a nejsou uložit kdekoli v rámci služby OMS.|
| **Adresa URL serveru**   | Zadejte adresu URL Cherwell instanci, kterou chcete připojit ke konektoru služby správy IT. |
| **ID klienta**   | Zadejte ID klienta pro toto připojení, který jste vygenerovali v instanci Cherwell ověřování.   |
| **Obor synchronizace dat**   | Vyberte Cherwell pracovní položky, které chcete synchronizovat prostřednictvím konektoru služby správy IT.  Tyto pracovní položky budou importovány do analýzy protokolů.   **Možnosti:** incidenty, žádosti o změnu. |
| **Synchronizaci dat** | Zadejte počet uplynulých dní, které chcete data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete vytvořit položky konfigurace v rámci ITSM produktu. Při výběru OMS vytvoří ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v podporovaných ITSM systému. **Výchozí**: zakázáno. |

Když se úspěšně připojil a synchronizovat:

- Vybrat pracovní položky z této Cherwell připojení jsou importovány do OMS analýzy protokolů. Můžete zobrazit souhrn těchto pracovní položky na dlaždici konektoru služby správy IT.
- Můžete vytvářet incidenty a události v této instanci Cherwell od OMS. Další informace: vytvoření ITSM pracovní položky pro výstrahy OMS a vytvořit ITSM pracovní položky z protokolů OMS.

Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="generate-client-id-for-cherwell"></a>Generování ID klienta pro Cherwell

Ke generování ID nebo klíč klienta pro Cherwell, použijte následující postup:

1. Přihlaste se k instanci Cherwell jako správce.
2. Klikněte na tlačítko **zabezpečení** > **nastavení klienta upravit REST API**.
3. Vyberte **vytvořit nového klienta** > **tajný klíč klienta**.

    ![Id uživatele Cherwell](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Další kroky
 - [Vytváření pracovních položek ITSM OMS výstrahy](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts)

 - [Vytváření pracovních položek ITSM z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs)

- [Zobrazení analýzy protokolů pro připojení k](log-analytics-itsmc-overview.md#using-the-solution)
