# ball-balancing-table

## 1. Introduzione :
BBTL è un progetto che mira a sviluppare una piattaforma in grado di bilanciare dinamicamente una sfera in movimento. L’obiettivo è controllare la rotazione, traslazione e rimbalzo della sfera sulla superficie, fino a stabilizzarla in una posizione prestabilita. Per il controllo del bilanciamento prevede lo sviluppo di algoritmi di ‘Edge AI‘ , per il tracciamento della posizione della palla, che vengono eseguiti in locale sul sensore IMU MEMS ISPU (intelligent sensor processing unit). La main board del BBTL (STM32-Nucleo) è responsabile di pilotare l’intera architettura elettronica della piattaforma , in più porta un modulo BLE che gli permette di interagire con un altro sistema di controllo .

## 2. Componenti HW e SW :

### Nucleo-F403RE :
- MCU : STM32F403RE
- SW and tool da usare :
  - STM32IDECube : IDE di sviluppo.
  - STM32CubeF4 .
        
### STEVAL-MKI229A : Board di sviluppo con sensore MEMS :
#### Sensore LSM6DSO16IS: 6-axis IMU (3 ACC + 3 GYRO) con ISPU :
- 3-axis accelerometer with selectable full scale.
- 3-axis gyroscope with selectable full scale .
- Embedded ISPU: ultra-low-power, high-performance programmable core to execute signal processing and AI algorithms in the edge for a seamless digital-life experience.
- IMU con microcontroller interno programmabile (ISPU) in C/assembly , per AI/ML custom (Edge-AI custom, sensor-hub master).
    -  ISPU = MCU RISC-32-bit con 8kB RAM.
  
#### SW e tool di sviluppo :
- MEMS-Studio.
- Unicleo-GUI.
- X-NUCLEO-MEMS1.
- X-CUBE-ISPU .
- ISPU-Toolchain.

#### Nota : 
La STEVAL-MKI229A non contiene alcun mcu dedicato per “flashare” il FW sulla ISPU (LSM6DSO16IS); Per fare ciò (caricare il file .ucf sulla ISPU) dobbiamo avere un MCU sterno che si possa connettere con il sensore ISPU attraverso I²C/SPI .

### X-NUCLEO-53L8A1 : Scheda di sviluppo con sensore ToF :
#### Sensore VL53L8CA :
- VL53L8CX Low-power high-performance 8x8 multizone ToF sensor.
- VL53L8CH Artificial intelligence enabler, high performance 8x8 multizone ToF sensor.

#### Sw da usare : X-CUBE-TOF1 .  
      
#### Nota : 
Per un controllo-loop che deve predire la traiettoria 3D della palla ogni ≤ 20 ms, VL53L8CX/CA vince con : 60 Hz nativi, latenza minore, API pronte; il VL53L8CH aggiunge l’output CNH per classificare oggetti/gesture, ma gira a 30 Hz e non implementa alcun modello di predizione onboard – la previsione resta comunque a carico dell’MCU ; VL53L8CA ha la stessa “personalizzazione” firmware del VL53L8CX .

### Dynamixel XL430-W250 : 
- Attuatori Bus-servo , pilota i movimenti della piattaforma.
  
### X-NUCLEO-BNRG2A1 : scheda con modulo BLE.
- Pacchetto SW BLE : X-Cube-BLE2

## 3. Descrizione della Applicazione :
BBLT è un sistema embedded di controllo attivo a ciclo chiuso che mantiene l’equilibrio di una sfera in tempo reale su una piattaforma, attraverso algoritmi di “Edge AI” che tendano di predire la posizione XY della sfera fondendo i dati forniti dai sensori IMU MEMS ISPU e ToF ; L’azione di controllo è applicata ai bus-servo Dynamixel XL430; L’elaboratore principale è il mcu STM32F401 che gestisce e sincronizza tutti i componenti della BBLT ; in più BBLT incorpora un modulo BLE per inviare e ricevere informazione con altri dispositivi .
      
