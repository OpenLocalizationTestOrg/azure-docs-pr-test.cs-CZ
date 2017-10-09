---
title: "připojení aaaITSM v OMS IT Service Management Connector | Microsoft Docs"
description: "Připojení ITSM produkty nebo služby s konektorem správy služeb IT v OMS toocentrally monitorování a správě hello ITSM pracovních položek."
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
ms.openlocfilehash: 53ef51bf75fb8ed15ea3ce5072d9365c221f9f4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-itsm-productsservices-with-it-service-management-connector-preview"></a>Připojení ITSM produkty nebo služby s IT Service Management Connector (Preview)
Tento článek obsahuje informace o tom, tooconnect tooIT produktům a službám vaší ITSM konektoru Management Service v OMS a centrálně spravovat pracovní položky. Další informace o konektoru služby správy IT, naleznete v [přehled](log-analytics-itsmc-overview.md).

podporovány jsou následující produkty nebo služby Hello:

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-oms)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-oms)
- [Provance](#connect-provance-to-it-service-management-connector-in-oms)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="connect-system-center-service-manager-tooit-service-management-connector-in-oms"></a>Připojit tooIT System Center Service Manager konektoru Management Service v OMS

Hello následující části obsahují podrobnosti o způsobu tooconnect vaše toohello produktu System Center Service Manager konektoru správy služeb IT v OMS.

### <a name="prerequisites"></a>Požadavky

Ujistěte se, že máte hello požadavky splněny následující:

- IT služby správy nainstalovaný konektor.
Další informace: [konfigurace](log-analytics-itsmc-overview.md#configuration).
- Hello aplikace webového portálu Service Manager (webové aplikace) je nasadit a nakonfigurovat. Informace o webové aplikace je [zde](#create-and-deploy-service-manager-web-app-service).
- Hybridní připojení vytvořený a nakonfigurovaný. Další informace: [konfigurovat hello hybridní připojení](#configure-the-hybrid-connection).
- Podporované verze portálu Service Manager: 2012 R2 nebo 2016.
- Role uživatele: [operátor s rozšířenými oprávněními](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Postup připojení

Použijte následující postup tooconnect hello vaší toohello System Center Service Manager instance konektoru služby správy IT:

1. Přejděte příliš**OMS** >**nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.

    ![Portál Service manager ](./media/log-analytics-itsmc/itsmc-service-manager-connection.png)
3. Zadejte hello informace, jak je popsáno v následující tabulce hello a klikněte na **Uložit** toocreate hello připojení:

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název pro instanci hello System Center Service Manager, které chcete tooconnect s hello konektoru služby správy IT.  Můžete použít tento název později při zobrazení analýzy podrobné protokolů nebo nakonfigurujte pracovních položek v této instanci. |
| **Vyberte typ připojení**   | Vyberte **System Center Service Manager**. |
| **Adresa URL serveru**   | Zadejte adresu URL hello hello portálu Service Manager webové aplikace. Další informace o portálu Service Manager webové aplikace je [zde](#create-and-deploy-service-manager-web-app-service).
| **ID klienta**   | Zadejte ID klienta hello, že jste vygenerovali (pomocí skriptu automatické hello) pro ověřování hello webové aplikace. Další informace o skriptu hello automatizované je [sem.](log-analytics-itsmc-service-manager-script.md)|
| **Tajný klíč klienta**   | Typ hello sdílený tajný klíč klienta, vygenerované číslem ID této.   |
| **Obor synchronizace dat**   | Vyberte hello portálu Service Manager pracovních položek, které chcete toosync prostřednictvím hello konektoru služby správy IT.  Tyto pracovní položky budou importovány do analýzy protokolů. **Možnosti:** incidenty, žádosti o změnu.|
| **Synchronizaci dat** | Zadejte hello počet uplynulých dní, které chcete hello data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete, aby toocreate hello položek konfigurace v produktu ITSM hello. Pokud vybraná, vytvoří OMS hello ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v hello podporovaný ITSM systému. **Výchozí**: zakázáno. |

Když se úspěšně připojil a synchronizovat:

- Vybrané pracovní položky z portálu Service Manager jsou importovány do OMS **analýzy protokolů.** Můžete zobrazit souhrn hello těchto pracovní položky na dlaždici hello konektoru služby správy IT.

- Od OMS můžete vytvořit incidenty z výstrah OMS nebo z protokolu hledání v této instanci portálu Service Manager.

Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Vytvoření a nasazení portálu Service Manager webové aplikace služby

tooconnect hello místní Service Manager s hello IT Service Management Connector na OMS, společnost Microsoft vytvořila portálu Service Manager webovou aplikaci na hello Githubu.

tooset hello ITSM webovou aplikaci pro portálu Service Manager, hello následující:

- **Webové aplikace nasadit hello** – nasazení hello webové aplikace, nastavte vlastnosti hello a ověřování s Azure AD. Hello webové aplikace můžete nasadit pomocí hello [automatizované skriptu](log-analytics-itsmc-service-manager-script.md) , společnost Microsoft poskytuje vám.
- **Konfigurace hello hybridní připojení** - [konfigurovat toto připojení](#configure-the-hybrid-connection), je nutné ručně.

#### <a name="deploy-hello-web-app"></a>Nasazení webové aplikace hello
Použití hello automatizované [skriptu](log-analytics-itsmc-service-manager-script.md) toodeploy hello webové aplikace, nastavte vlastnosti hello a ověřování s Azure AD.

Spusťte skript hello tím, že poskytuje hello následující požadované podrobnosti:

- Podrobnosti o předplatném Azure
- Název skupiny prostředků
- Umístění
- Podrobnosti o serveru portálu Service Manager (název serveru, domény, uživatelské jméno a heslo)
- Předpona názvu lokality pro vaši webovou aplikaci
- Namespace sběrnice.

Hello skript vytvoří hello webovou aplikaci pomocí hello název, který jste zadali (spolu s několika toomake další řetězce je jedinečný). Vygeneruje hello **adresa URL webové aplikace**, **ID klienta** a **tajný klíč klienta**.

Uložte hello hodnoty je používat při vytváření připojení ke konektoru služby správy IT.

**Kontrola instalace hello webové aplikace**

1. Přejděte příliš**portál Azure** > **prostředky**.
2. Vyberte hello webové aplikace, klikněte na **nastavení** > **nastavení aplikace**.
3. Potvrďte hello informace o instanci hello portálu Service Manager, který jste zadali v době hello nasazení aplikace hello prostřednictvím hello skriptu.

### <a name="configure-hello-hybrid-connection"></a>Konfigurace hello hybridní připojení

Použijte následující postup tooconfigure hello hybridní připojení, která se připojuje instanci portálu Service Manager hello s hello konektoru správy služeb IT v OMS hello.

1. Najde hello portálu Service Manager webové aplikace, v části **prostředky Azure**.
2. Klikněte na tlačítko **nastavení** > **sítě**.
3. V části **hybridní připojení**, klikněte na tlačítko **nakonfigurovat koncové body hybridního připojení**.

    ![Hybridní připojení sítě](./media/log-analytics-itsmc/itsmc-hybrid-connection-networking-and-end-points.png)
4. V hello **hybridní připojení** okně klikněte na tlačítko **přidat hybridní připojení**.

    ![Přidat hybridní připojení.](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-add.png)

5. V hello **přidat hybridní připojení** okně klikněte na tlačítko **vytvořit nové hybridní připojení**.

    ![Nové hybridní připojení](./media/log-analytics-itsmc/itsmc-create-new-hybrid-connection.png)

6. Zadejte hello následující hodnoty:

    - **Název koncového bodu**: Zadejte název pro hello nové hybridní připojení.
    -  **Koncový bod hostitele**: plně kvalifikovaný název domény serveru pro správu portálu Service Manager hello.
    - **Koncový Port**: Zadejte 5724
    - **Obor názvů sběrnice**: použití existujícího oboru názvů sběrnice nebo vytvořte novou.
    - **Umístění**: Vyberte umístění hello.
    -  **Název**: při jeho vytváření zadejte sběrnice toohello název.

    ![Hybridní připojení hodnoty](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-values.png)
6. Klikněte na tlačítko **OK** tooclose hello **vytvořit hybridní připojení** okno a spusťte vytvoření hello hybridní připojení.

    Po vytvoření hello hybridní připojení se zobrazí v okně hello.

7. Po vytvoření hello hybridní připojení, vyberte hello připojení a klikněte na tlačítko **přidat vybrané hybridní připojení**.

    ![Nové hybridní připojení](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-hello-listener-setup"></a>Konfigurace instalačního programu hello naslouchací proces

Použijte následující postup tooconfigure hello naslouchací proces instalace pro hybridní připojení hello hello.

1. V hello **hybridní připojení** okně klikněte na tlačítko **stažení hello Správce připojení** a nainstalujte ji na hello počítači, kde je spuštěna instance System Center Service Manager.

    Po dokončení instalace hello **uživatelského rozhraní správce hybridního připojení** možnost je k dispozici v části **spustit** nabídky.

2. Klikněte na tlačítko **uživatelského rozhraní správce hybridního připojení** , zobrazí se výzva k zadání pověření Azure.

3. Přihlášení pomocí svých přihlašovacích údajů Azure a vybrat své předplatné, které bylo vytvořeno hello hybridní připojení.

4. Klikněte na **Uložit**.

Hybridní připojení se úspěšně připojil.

![úspěšné hybridní připojení.](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Po vytvoření hello hybridní připojení ověřte a test připojení hello návštěvou hello nasazení portálu Service Manager webové aplikace. Zkontrolujte, zda text hello připojení je úspěšné, než se pokusíte tooconnect toohello konektoru správy služeb IT v OMS.

Hello následující obrázek ukazuje hello podrobnosti o úspěšném připojení:

![Test hybridní připojení](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-tooit-service-management-connector-in-oms"></a>Připojení ServiceNow tooIT konektoru Management Service v OMS

Hello následující části obsahují podrobnosti o způsobu tooconnect vaše ServiceNow produktu toohello konektoru správy služeb IT v OMS.

### <a name="prerequisites"></a>Požadavky

Ujistěte se, že máte hello požadavky splněny následující:

- IT služby správy nainstalovaný konektor. Další informace: [konfigurace.](log-analytics-itsmc-overview.md#configuration)
- ServiceNow podporována verze – Fuji, Genevy, Helsinkách.

Správci ServiceNow musíte udělat následující v jejich instance ServiceNow hello:
- Generování ID klienta a tajný klíč klienta pro produkt ServiceNow hello. Informace o tom, jak toogenerate ID klienta a tajný klíč, najdete v části [instalace OAuth](http://wiki.servicenow.com/index.php?title=OAuth_Setup).
- Nainstalujte hello uživatele aplikace pro integraci Microsoft OMS (ServiceNow aplikace). [Další informace](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0 ).
- Umožňuje vytvořte roli uživatele integrace pro nainstalované aplikace hello uživatele. Informace o tom, jak role uživatele integrace toocreate hello je [zde](#create-integration-user-role-in-servicenow-app).


### <a name="connection-procedure"></a>**Postup připojení**

Použijte následující postup toocreate připojení ServiceNow hello:

1. Přejděte příliš**OMS** > **nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.

    ![Připojení ServiceNow](./media/log-analytics-itsmc/itsmc-servicenow-connection.png)

3. Zadejte hello informace, jak je popsáno v následující tabulce hello a klikněte na **Uložit** toocreate hello připojení:

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název instance ServiceNow hello, které chcete tooconnect s hello konektoru služby správy IT.  Tento název použijete později v OMS při zobrazení analýzy podrobné protokolů nebo na tento ITSM nakonfigurujte pracovních položek. |
| **Vyberte typ připojení**   | Vyberte **ServiceNow**. |
| **Uživatelské jméno**   | Zadejte hello integrace uživatelské jméno, který jste vytvořili v hello ServiceNow aplikace toosupport hello připojení toohello konektoru služby správy IT. Další informace: [ServiceNow vytvořit roli uživatele aplikace](#create-integration-user-role-in-servicenow-app).|
| **Heslo**   | Zadejte hello heslo přidružené k této uživatelské jméno. **Poznámka:**: uživatelské jméno a heslo pro generování pouze tokeny ověřování se používají a nejsou uložit kdekoli v rámci služby OMS hello.  |
| **Adresa URL serveru**   | Zadejte adresu URL instance ServiceNow hello, které chcete tooconnect tooIT konektoru Service Management hello. |
| **ID klienta**   | Zadejte ID klienta hello, který chcete použít pro ověřování OAuth2, který jste vygenerovali dříve toouse.  Další informace o generování ID klienta a tajný klíč: [instalace OAuth](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Tajný klíč klienta**   | Typ hello sdílený tajný klíč klienta, vygenerované číslem ID této.   |
| **Obor synchronizace dat**   | Vyberte hello ServiceNow pracovních položek, které chcete toosync tooOMS prostřednictvím hello konektoru služby správy IT.  Hello vybrané hodnoty jsou importovány do analýzy protokolů.   **Možnosti:** incidenty a žádosti o změnu.|
| **Synchronizaci dat** | Zadejte hello počet uplynulých dní, které chcete hello data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete, aby toocreate hello položek konfigurace v produktu ITSM hello. Pokud vybraná, vytvoří OMS hello ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v hello podporovaný ITSM systému. **Výchozí**: zakázáno. |


Když se úspěšně připojil a synchronizovat:

- Vybrat pracovní položky z připojení ServiceNow jsou importovány do OMS Log Analytics.  Můžete zobrazit souhrn hello těchto pracovní položky na dlaždici hello konektoru služby správy IT.
- Incidenty, výstrahy a událostí můžete vytvořit z OMS výstrahy nebo protokolu hledání u této instance ServiceNow.  


Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-integration-user-role-in-servicenow-app"></a>Umožňuje vytvořit roli uživatele integrace v aplikaci ServiceNow

Uživatel hello následující postup:

1.  Navštivte hello [ServiceNow úložiště](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0) a nainstalujte hello **uživatele aplikace pro integraci produktů Microsoft OMS a ServiceNow** do vaší Instance ServiceNow.
2.  Po instalaci navštivte hello levé navigační panel hello ServiceNow instance, vyhledávání a vyberte integrátor Microsoft OMS.  
3.  Klikněte na tlačítko **kontrolní seznam pro instalaci**.

    Zobrazí se stav Hello jako **nedokončila** Pokud hello role uživatele je ještě toobe vytvořit.

4.  V textu hello oknech, další příliš**uživatel integrace vytvořit**, zadejte hello uživatelské jméno pro uživatele hello, zda se může připojit toohello konektoru správy služeb IT v OMS.
5.  Zadejte hello heslo pro tohoto uživatele a klikněte na tlačítko **OK**.  

>[!NOTE]

> Tyto přihlašovací údaje toomake hello ServiceNow připojení použijete v OMS.

Zobrazí se hello výchozí role přiřazené se Hello nově vytvořený uživatel.

Výchozí role:
- personalize_choices
- import_transformer
-   x_mioms_microsoft.User
-   ITIL
-   template_editor
-   view_changer

Po úspěšném vytvoření uživatele hello hello stav **zkontrolujte kontrolní seznam instalace** přesune tooCompleted, výpis hello podrobnosti hello role uživatele pro aplikace hello vytvořit.

> [!NOTE]

> tooallow uživatele toocreate **výstrahy** a **události** v ServiceNow od OMS:

> - Zkontrolujte, zda že máte hello správě událostí modulu nainstalovaná na vaší instance ServiceNow.

> - Přidejte následující role uživatele integrace toohello hello:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-tooit-service-management-connector-in-oms"></a>Připojit Provance tooIT konektoru Management Service v OMS

Hello následující části obsahují podrobnosti o způsobu tooconnect vaše Provance produktu toohello konektoru správy služeb IT v OMS.

### <a name="prerequisites"></a>Požadavky

Ujistěte se, že máte hello požadavky splněny následující:

- IT služby správy nainstalovaný konektor. Další informace: [konfigurace](log-analytics-itsmc-overview.md#configuration).
- Provance aplikace by měla být zaregistrován u služby Azure AD - a ID klienta je k dispozici. Podrobné informace najdete v tématu [jak ověřování služby active directory tooconfigure](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
- Role uživatele: správce.

### <a name="connection-procedure"></a>Postup připojení

Použijte následující postup toocreate připojení Provance hello:

1. Přejděte příliš**OMS** > **nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.  

    ![Provance připojení](./media/log-analytics-itsmc/itsmc-provance-connection.png)
3. Zadejte hello informace, jak je popsáno v následující tabulce hello a klikněte na **Uložit** toocreate hello připojení.

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název pro instanci Provance hello, které chcete tooconnect s hello konektoru služby správy IT.  Tento název použijete později v OMS při zobrazení analýzy podrobné protokolů nebo na tento ITSM nakonfigurujte pracovních položek. |
| **Vyberte typ připojení**   | Vyberte **Provance**. |
| **Uživatelské jméno**   | Zadejte uživatelské jméno hello, zda se může připojit toohello konektoru služby správy IT.    |
| **Heslo**   | Zadejte hello heslo přidružené k této uživatelské jméno. **Poznámka:** uživatelské jméno a heslo pro generování pouze tokeny ověřování se používají a nejsou uložit kdekoli v rámci služby OMS hello. _|
| **Adresa URL serveru**   | Zadejte adresu URL hello vaší Provance instance, které chcete tooconnect tooIT konektoru Management Service. |
| **ID klienta**   | Zadejte ID klienta hello pro toto připojení, který jste vygenerovali v instanci Provance ověřování.  Další informace o ID klienta, viz [jak ověřování služby active directory tooconfigure](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Obor synchronizace dat**   | Vyberte hello Provance pracovních položek, které chcete toosync tooOMS prostřednictvím hello konektoru služby správy IT.  Tyto pracovní položky budou importovány do analýzy protokolů.   **Možnosti:** incidenty, žádosti o změnu.|
| **Synchronizaci dat** | Zadejte hello počet uplynulých dní, které chcete hello data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete, aby toocreate hello položek konfigurace v produktu ITSM hello. Pokud vybraná, vytvoří OMS hello ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v hello podporovaný ITSM systému. **Výchozí**: zakázáno.|

Když se úspěšně připojil a synchronizovat:

- Vybrané pracovní položky z Provance připojení jsou importovány do OMS **analýzy protokolů.**  Můžete zobrazit souhrn hello těchto pracovní položky na dlaždici hello konektoru služby správy IT.
- V této instanci Provance, můžete vytvořit incidenty a události z OMS výstrahy nebo hledání protokolů.

Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

## <a name="connect-cherwell-tooit-service-management-connector-in-oms"></a>Připojit Cherwell tooIT konektoru Management Service v OMS

Hello následující části obsahují podrobnosti o způsobu tooconnect vaše Cherwell produktu toohello konektoru správy služeb IT v OMS.

### <a name="prerequisites"></a>Požadavky

Ujistěte se, že máte hello požadavky splněny následující:

- IT služby správy nainstalovaný konektor. Další informace: [konfigurace](log-analytics-itsmc-overview.md#configuration).
- Vygeneruje ID klienta. Další informace: [generování ID klienta pro Cherwell](#generate-client-id-for-cherwell).
- Role uživatele: správce.

### <a name="connection-procedure"></a>Postup připojení

Použijte následující postup toocreate připojení Cherwell hello:

1. Přejděte příliš**OMS** >  **nastavení** > **připojené zdroje**.
2. Vyberte **ITSM konektor** klikněte na tlačítko **nové připojení**.  

    ![Id uživatele Cherwell](./media/log-analytics-itsmc/itsmc-cherwell-connection.png)

3. Zadejte hello informace, jak je popsáno v následující tabulce hello a klikněte na **Uložit** toocreate hello připojení.

> [!NOTE]
> Všechny tyto parametry jsou povinné.

| **Pole** | **Popis** |
| --- | --- |
| **Název**   | Zadejte název pro instanci Cherwell hello, které chcete tooconnect toohello konektoru služby správy IT.  Tento název použijete později v OMS při zobrazení analýzy podrobné protokolů nebo na tento ITSM nakonfigurujte pracovních položek. |
| **Vyberte typ připojení**   | Vyberte **Cherwell.** |
| **Uživatelské jméno**   | Zadejte uživatelské jméno Cherwell hello, zda se může připojit toohello konektoru služby správy IT. |
| **Heslo**   | Zadejte hello heslo přidružené k této uživatelské jméno. **Poznámka:** uživatelské jméno a heslo pro generování pouze tokeny ověřování se používají a nejsou uložit kdekoli v rámci služby OMS hello.|
| **Adresa URL serveru**   | Zadejte adresu URL hello vaší Cherwell instance, které chcete tooconnect tooIT konektoru Management Service. |
| **ID klienta**   | Zadejte ID klienta hello pro toto připojení, který jste vygenerovali v instanci Cherwell ověřování.   |
| **Obor synchronizace dat**   | Vyberte hello Cherwell pracovních položek, které chcete toosync prostřednictvím hello konektoru služby správy IT.  Tyto pracovní položky budou importovány do analýzy protokolů.   **Možnosti:** incidenty, žádosti o změnu. |
| **Synchronizaci dat** | Zadejte hello počet uplynulých dní, které chcete hello data z. **Maximální limit**: 120 dní. |
| **Vytvořit novou položku konfigurace v ITSM řešení** | Tuto možnost vyberte, pokud chcete, aby toocreate hello položek konfigurace v produktu ITSM hello. Pokud vybraná, vytvoří OMS hello ovlivněných položek konfigurace jako položky konfigurace (v případě neexistující položek konfigurace) v hello podporovaný ITSM systému. **Výchozí**: zakázáno. |

Když se úspěšně připojil a synchronizovat:

- Vybrat pracovní položky z této Cherwell připojení jsou importovány do OMS analýzy protokolů. Můžete zobrazit souhrn hello těchto pracovní položky na dlaždici hello konektoru služby správy IT.
- Můžete vytvářet incidenty a události v této instanci Cherwell od OMS. Další informace: vytvoření ITSM pracovní položky pro výstrahy OMS a vytvořit ITSM pracovní položky z protokolů OMS.

Další informace: [ITSM vytvoření pracovní položky pro výstrahy OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) a [ITSM vytvoření pracovní položky z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="generate-client-id-for-cherwell"></a>Generování ID klienta pro Cherwell

Klient hello toogenerate ID nebo klíč pro Cherwell hello použijte následující postup:

1. Přihlaste se tooyour Cherwell instance jako správce.
2. Klikněte na tlačítko **zabezpečení** > **nastavení klienta upravit REST API**.
3. Vyberte **vytvořit nového klienta** > **tajný klíč klienta**.

    ![Id uživatele Cherwell](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Další kroky
 - [Vytváření pracovních položek ITSM OMS výstrahy](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts)

 - [Vytváření pracovních položek ITSM z protokolů OMS](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs)

- [Zobrazení analýzy protokolů pro připojení k](log-analytics-itsmc-overview.md#using-the-solution)
