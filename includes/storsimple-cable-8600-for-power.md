<!--author=alkohli last changed: 9/16/15-->


#### <a name="toocable-your-device-for-power"></a>toocable vašeho zařízení z hlediska výkonu
> [!NOTE]
> Obě skříně zařízení StorSimple zahrnují redundantní PCMs. Pro každou skříň hello PCMs musí být nainstalovaný a připojený toodifferent power zdroje tooensure vysokou dostupnost.
> 
> 

1. Zajistěte, aby hello napájení přepínače na všechny PCMs hello byla v pozici hello OFF.
2. Na hello primární skříň připojte hello power kabelů tooboth PCMs. Hello napájecích kabelů jsou identifikovány červeně hello power kabelů diagramu níže.
3. Ujistěte se, který hello dvě PCMs na hello primární skříň použití samostatných power zdroje.
4. Připojte hello power kabelů toohello zapnutí jednotek pro distribuci rack hello jak je znázorněno v hello power kabelů diagram.
5. Opakujte kroky 2 až 4 pro hello EBOD skříň.
6. Zapněte hello EBOD skříň překlopení hello vypínač na každý PCM toohello ON pozici.
7. Ověřte, že tento hello EBOD skříň zapnutý kontrolou, že jsou hello zelená LED na hello zadní hello EBOD řadiče povolena ON.
8. Zapněte primární skříň hello překlopení každý PCM přepínač toohello ON pozici.
9. Ověřte, že hello systému je na zajištěním řadiče zařízení hello, které jste zapnuli LED.
10. Ujistěte se, že hello připojení mezi řadiči EBOD hello a řadiče zařízení hello je aktivní pomocí ověření, že budou zelené hello čtyři LED další toohello SAS portů na řadiči EBOD hello.
    
    > [!IMPORTANT]
    > tooensure vysoká dostupnost pro váš systém, doporučujeme striktně dodržovat toohello power kabelů hello následující diagram znázorňuje schéma.
    > 
    > 
    
    ![Zapojení kabeláže zařízení 4U pro napájení](./media/storsimple-cable-8600-for-power/HCSCableYour4UDeviceforPower.png)
    
    **Kabeláž napájení**
    
    | Štítek | Popis |
    |:--- |:--- |
    | 1 |Primární skříň |
    | 2 |PCM 0 |
    | 3 |PCM 1 |
    | 4 |Řadič 0 |
    | 5 |Řadič 1 |
    | 6 |EBOD řadič 0 |
    | 7 |EBOD řadiči 1 |
    | 8 |EBOD skříň |
    | 9 |Jednotky PDU |

