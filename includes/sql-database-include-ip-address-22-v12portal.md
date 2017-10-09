
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, hello following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through hello new Azure portal
-->


1. Přihlaste se toohello [portál Azure](https://portal.azure.com/) v http://portal.azure.com/.
2. V levém banner hello, klikněte na **Procházet vše**. Hello **Procházet** zobrazí se okno.
3. Přejděte a klikněte na **servery SQL**. Hello **servery SQL** zobrazí se okno.
   
    ![V aplikaci portál hello najít server služby Azure SQL Database][b21-FindServerInPortal]
4. Pro usnadnění práce, klikněte na tlačítko hello minimalizovat ovládací prvek v dříve hello **Procházet** okno.
5. Do textového pole hello filtr začněte zadávat text hello název vašeho serveru. Řádek, který jste se zobrazí.
6. Klikněte na řádek hello pro váš server. Zobrazí se okno pro váš server.
7. V okně vaší serveru klikněte na tlačítko **nastavení**. Hello **nastavení** zobrazí se okno.
8. Klikněte na tlačítko **brány Firewall**. Hello **nastavení brány Firewall** zobrazí se okno.
   
    ![Klikněte na Nastavení > brány Firewall][b31-SettingsFirewallNavig]
9. Klikněte na tlačítko **Přidání klienta IP**. Zadejte název pro nové pravidlo do textového pole pro první hello.
10. Typ v hello nízkou a vysokou IP adres hodnoty pro rozsah hello chcete tooenable.
    
    * Může být užitečný toohave hello nízkou hodnotu končit **.0** a hello vysoké s **.255**.
    
    ![Přidat tooallow rozsah adres IP][b41-AddRange]
11. Klikněte na **Uložit**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
