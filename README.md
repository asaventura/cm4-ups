# cm4-ups
Rpi cm4 ups project

graph TD
    subgraph Входная цепь (20В по запросу)
        PSU[БП USB-C PD 65W+] --> PD_T[PD-триггер <br> Запрос 20В]
    end

    subgraph Цепь быстрой зарядки
        PD_T -- 20В --> CHARGER[Высокотоковая <br> плата заряда 3S] --> BMS(3S BMS Плата)
    end
    
    subgraph Цепь питания Pi (12В)
        PD_T -- 20В --> BUCK_12V[Понижающий DC-DC <br> 20В -> 12В]
        BUCK_12V -- 12В --> MOSFET{P-channel <br> MOSFET Switch}
        BMS -- 9В-12.6В --> MOSFET
        MOSFET --> OUT_BUCKBOOST[Buck-Boost DC-DC <br> Выход = 12В] --> RPI[CM4 I/O Board]
    end

    subgraph Аккумуляторный блок
        LIPO[Li-Po 3S] --> BMS
    end

    subgraph Контроль разряда
        BMS -- Отслеживание V_bat --> ADC[АЦП ADS1115] -- I2C --> RPI
    end

    style PSU fill:#cde4ff
    style RPI fill:#ffcdd2
