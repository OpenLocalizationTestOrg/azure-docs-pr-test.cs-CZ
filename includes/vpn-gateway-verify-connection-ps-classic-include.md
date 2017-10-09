Můžete ověřit, že bylo připojení úspěšně pomocí rutiny hello 'Get-AzureVNetConnection'.

1. Hello použijte následující příklad rutiny, konfigurace toomatch hello hodnoty vlastními. Název Hello hello virtuální sítě musí být v uvozovkách, pokud obsahuje mezery.

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Po dokončení rutiny hello zobrazte hello hodnoty. V příkladu hello níže zobrazuje stav připojení hello jako "Připojena" a vidíte příchozí a Odchozí bajty.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : hello connectivity state for hello local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal