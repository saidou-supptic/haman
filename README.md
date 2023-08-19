	Creation of the WIFI access point.
name = "testWifi
pwd = "password
def CreateWifiConfig ( SSID , password ):
        config_lines = [
        'ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev' , 'update_config=1' , 'country=CMR' , ' \n ' , 'network={' ,' \t ssid="{}"' . format ( SSID ), ' \ tpsk="{}"' . format ( password ), '}'
        ]
CreateWifiConfig ( name , password )
os . popen ( "sudo reboot" )

	Creating and configuring the Bluetooth access point
def receiveMessages () :
  server_sock = bluetooth .BluetoothSocket ( bluetooth . RFCOMM )
  
def sendMessageTo ( targetBluetoothMacAddress ) :
  port = 1
  sock = bluetooth . BluetoothSocket ( bluetooth . RFCOMM )
  sock . connect (( targetBluetoothMacAddress , port ))
  sock . send ("bonjour ! !" )
  sock . close ()
  
def lookUpNearbyBluetoothDevices () :
  appareils_aux alentours = bluetooth . discover_devices ()
  for bdaddr in near_devices :
    print str ( bluetooth . lookup_name ( bdaddr )) + " [" + str ( bdaddr ) + "]"

	Connecting the Lora server module to the Raspberry pi
classe mylora ( LoRa ) :
    def __init__ ( self , verbose = False ) :
        super ( mylora , this ). __init__ ( verbose )
        self . set_mode ( MODE.sleep )
        self. set_dio_mapping ([ 0 ] * 6 )
        self . var = 0
    def start(self) :          
        while True :
            while (self.var==0) :
                print ("Send : INF")
                self . write_payload([255, 255, 0, 0, 73, 78, 70, 0]) 
                self.set_mode(MODE.TX)
                time . sleep ( 3 ) 
                self . reset_ptr_rx ()
                self . set_mode ( MODE . RXCONT ) 
lora = mylora ( verbose = Faux )
lora . set_pa_config ( pa_select = 1 , max_power = 21 , output_power = 15 )
lora . set_bw ( BW . BW125 )
lora . set_coding_rate ( CODING_RATE . CR4_8 )
lora . set_spreading_factor ( 12 )
lora . set_rx_crc ( Vrai )
lora . set_low_data_rate_optim ( Vrai )
assert( lora . get_agc_auto_on () == 1 )

	Configuring and connecting the LoRa client module to the server
BOARD.setup()
BOARD.init()
BOARD . install ()
BOARD.init ()
classe mylora ( LoRa ) :
    def __init__ ( self , verbose = False ) :
        super ( mylora , soi ). __init__ ( verbose )
        self . set_mode ( MODE .sleep)
        self . set_dio_mapping ([ 0 ] * 6 )
    def on_rx_done ( self ) :
        BOARD . led_on ()
        self . clear_irq_flags ( RxDone = 1 )
        payload = self . read_payload ( nocheck = True ) # Recevoir INF
        print ("Recevoir : " )
        mens = octets ( charge utile ). décoder ( "utf-8" , 'ignorer' )
        mens = mens [ 2 : - 1 ] #pour supprimer \x00\x00 et \x00 à la fin
        print(mens)
        BOARD.led_off()
        if mens=="INF" :
            print("Received data request INF")
            time.sleep(2)
            print ("Send mens : DATA RASPBERRY PI")
            self.write_payload([255, 255, 0, 0, 68, 65, 84, 65, 32, 82, 65, 83, 80, 66, 69, 82, 82, 89, 32, 80, 73, 0]) # Envoyer les données RASPBERRY PI
            self.set_mode(MODE.TX)
        time . sleep ( 2 )
        self . reset_ptr_rx ()
        self . set_mode ( MODE . RXCONT )    
    def on_tx_done ( self ) :
        print (" \n TxFait" )
        print ( self . get_irq_flags ())

def start(self) :          
        while True :
            self.reset_ptr_rx()
            self.set_mode(MODE.RXCONT) # Mode récepteur
            while True :
                pass ;
lora = mylora(verbose=False)

lora.set_pa_config(pa_select=1, max_power=21, output_power=15)
lora.set_bw(BW.BW125)
lora.set_coding_rate(CODING_RATE.CR4_8)
lora.set_spreading_factor(12)
lora.set_rx_crc(True)
lora.set_low_data_rate_optim(True)
assert(lora.get_agc_auto_on() == 1)
