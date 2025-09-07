### Printer.cfg с комментариями

Версия 4.4.24
Для детальной справки по намтройкам можно посмотреть в официальной документации [Klipper](https://www.klipper3d.org/Config_Reference.html)

```ini
# V4.4.14 2024-3-14
# update: heater_generic hot -> chamber
#         out_put_pin fan0 -> fan_generic cooling_fan
#         out_put_pin fan2 -> fan_generic auxiliary_cooling_fan
#         out_put_pin fan3 -> fan_generic chamber_circulation_fan
#         heater_fan hot -> heater_fan chamber
#         verify_heater hot -> verify_heater chamber
# V4.4.17 2024-3-29
# update: delete comments
#         add time_update macro
#         [bed_mesh] 6,6 ->8,8
# V4.4.19 2024-4-16
# update: add [chamber_fan chamber_fan]
# V4.4.20 2024-6-17
# update: [smart_effector]
#           samples_result: average -> submaxmin
#           speed: 10 -> 5
#           sample_retract_dist: 3.0 -> 5.0
#           [stepper_x]
#           position_max: 245 -> 246

# Включение других файлов конфигурации для модульности
[include timelapse.cfg] 
[include Adaptive_Mesh.cfg] # Настройки для адаптивной калибровки стола
[include gcode_macro.cfg] # Пользовательские макросы G-кода
[include plr.cfg] 
[include time_update.cfg]

# Настройки первого контроллера (на основной плате)
[mcu]
# Путь к последовательному порту, к которому подключена плата
serial: /dev/ttyS2 
restart_method: command

# Настройки второго контроллера (плата головы)
[mcu U_1]
# Путь к последовательному порту дополнительной платы
serial: /dev/ttyS0 
restart_method: command

# Отправка сообщений в интерфейс Klipper (консоль)
[respond]
# Устанавливает префикс для вывода команд M118 и RESPOND.
default_type: echo 

# Сохранение переменных Klipper в файл
[save_variables]
filename =/home/mks/klipper_config/saved_variables.cfg

# Настройки для теста резонансов
[resonance_tester]
# Определяет ускорение для теста конкретной частоты (accel = accel_per_hz * freq).
accel_per_hz: 150
# Максимальное сглаживание для входного шейпера.
max_smoothing:0.5

# Переопределение пинов, чтобы использовать один и тот же пин в нескольких секциях
[duplicate_pin_override]
pins:
# Переопределение пина gpio21 на U_1:PC3
gpio21 ,U_1:PC3 

# Настройки для ручной калибровки стола с помощью винтов
[bed_screws]
# Координаты первого винта
screw1:10,10
# Имя винта
screw1_name: Front left
screw2: 230,10
screw2_name: Front right 
screw3: 125,240 
screw3_name: Last right 

# Настройки для автоматической регулировки наклона стола
[screws_tilt_adjust]
screw1:-5,5.6 
screw1_name: Front left 
screw2: 216,5.6 
screw2_name: Front right 
screw3: 103,235.6 
screw3_name: Last right 
screw_thread: CW-M4 # Тип резьбы винтов для расчета углов поворотов

# Управление принудительным перемещением моторов
[force_move]
enable_force_move : false # Если True, позволяет использовать команды FORCE_MOVE и SET_KINEMATIC_POSITION.

# Датчик ширины филамента на основе эффекта Холла (у нас работает как датчик окончания филамента, расположен в голове над фидером)
[hall_filament_width_sensor]
adc1: gpio27 # Аналоговый входной пин, подключенный к сенсору.
adc2: gpio28 # Аналоговый входной пин, подключенный к сенсору.
cal_dia1: 1.50 # Калибровочное значение диаметра (в мм).
cal_dia2: 2.0 # Калибровочное значение диаметра (в мм).
raw_dia1: 14397 # Необработанное калибровочное значение.
raw_dia2: 15058 # Необработанное калибровочное значение.
default_nominal_filament_diameter: 1.75 # Номинальный диаметр филамента по умолчанию.
max_difference: 0 # Максимально допустимая разница в диаметре (в мм).
measurement_delay: 50 # Расстояние от датчика до хотэнда (в мм).
enable: false # Включен или выключен датчик после запуска.
measurement_interval: 10 # Приблизительное расстояние (в мм) между измерениями.
logging: False # Включить/выключить вывод данных в консоль и лог-файл.
min_diameter: 0.3 # Минимальный диаметр для срабатывания виртуального датчика.
use_current_dia_while_delay: False # Использовать текущий диаметр вместо номинального во время задержки.
pause_on_runout:True # Если True, принтер остановится сразу после обнаружения отсутствия филамента.
runout_gcode:
    RESET_FILAMENT_WIDTH_SENSOR # G-код, выполняемый при обнаружении проблемы.
    M118 Filament run out # Сообщение для консоли.
event_delay: 3.0 # Минимальное время между событиями.
pause_delay: 0.5 # Время задержки (в секундах) между командой паузы и выполнением G-кода.

# Настройки экструдера
[extruder]
step_pin:gpio5 # Пин для шагов.
dir_pin:gpio4 # Пин для направления.
enable_pin:!gpio10 # Пин для включения драйвера (инвертирован).
rotation_distance: 53.7 # Расстояние (в мм), которое филамент проходит за один полный оборот мотора.
gear_ratio: 1517:170 # Передаточное отношение редуктора.
microsteps: 16 # Количество микрошагов.
full_steps_per_rotation: 200 # Количество полных шагов на один оборот мотора.
nozzle_diameter: 0.400 # Диаметр сопла.
filament_diameter: 1.75 # Номинальный диаметр филамента.
min_temp: 0 # Минимальная безопасная температура.
max_temp: 360 # Максимальная безопасная температура.
min_extrude_temp: 175 # Минимальная температура для экструзии.
smooth_time: 0.000001 # Время сглаживания.
heater_pin:gpio24 # Пин, управляющий нагревателем.
sensor_type:MAX6675 # Тип температурного сенсора.
sensor_pin:gpio17 # Аналоговый пин, подключенный к сенсору.
spi_speed: 100000 # Скорость SPI.
spi_software_sclk_pin:gpio18 # Пин для тактового сигнала SPI.
spi_software_mosi_pin:gpio19 # Пин для передачи данных SPI.
spi_software_miso_pin:gpio16 # Пин для получения данных SPI.
max_power: 1 # Максимальная мощность нагревателя (от 0.0 до 1.0).
control : pid # Алгоритм управления нагревом.
pid_Kp=33.555 # Пропорциональный коэффициент PID.
pid_Ki=4.76 # Интегральный коэффициент PID.
pid_Kd=59.141 # Дифференциальный коэффициент PID.
pressure_advance: 0.032 # Компенсация давления.
pressure_advance_smooth_time: 0.03 # Время сглаживания для компенсации давления.
max_extrude_cross_section:500 # Максимальная площадь сечения экструзии.
instantaneous_corner_velocity: 10.000 # Максимальная мгновенная скорость изменения экструдера на углу.
max_extrude_only_distance: 1000.0 # Максимальная длина филамента для ретракта.
max_extrude_only_velocity:5000 # Максимальная скорость ретракта.
max_extrude_only_accel:2000 # Максимальное ускорение ретракта.
step_pulse_duration:0.000002 # Минимальное время между импульсами шага.

# Конфигурация драйвера TMC2209 для экструдера через UART
[tmc2209 extruder]
uart_pin:gpio6 # Пин для UART-связи.
interpolate: True # Включение интерполяции микрошагов.
run_current: 0.714 # Ток (в амперах RMS) для движения.
stealthchop_threshold: 0 # Скорость (в мм/с) для включения StealthChop. 0 - выключен.

# Настройки акселерометра ADXL345
[adxl345]
cs_pin:gpio13 # Пин выбора чипа.
spi_software_sclk_pin:gpio14 # Пин для тактового сигнала SPI.
spi_software_mosi_pin:gpio15 # Пин для передачи данных SPI.
spi_software_miso_pin:gpio12 # Пин для получения данных SPI.
axes_map: -x, z, -y # Переназначение осей акселерометра.

# Высокоуровневые настройки принтера
[printer]
kinematics:corexy # Тип кинематики принтера.
max_velocity: 600 # Максимальная скорость.
max_accel: 20000 # Максимальное ускорение.
max_accel_to_decel: 10000 # Ускорение для замедления.
max_z_velocity: 10 # Максимальная скорость по оси Z.
max_z_accel: 500 # Максимальное ускорение по оси Z.
square_corner_velocity: 8 # Максимальная скорость на углу 90 градусов.

# Настройки шагового двигателя оси X
[stepper_x]
step_pin:U_1:PB4 # Пин для шагов.
dir_pin:!U_1:PB3 # Пин для направления (инвертирован).
enable_pin:!U_1:PB5 # Пин для включения (инвертирован).
microsteps:16 # Количество микрошагов.
rotation_distance: 39.88 # Расстояние (в мм), которое ось проходит за один полный оборот мотора.
full_steps_per_rotation:200 # Количество полных шагов на оборот.
endstop_pin:tmc2240_stepper_x:virtual_endstop # Пин концевика (виртуальный, на основе драйвера).
position_min: -5.5 # Минимальная допустимая позиция.
position_endstop: -5.5 # Позиция концевика.
position_max:246 # Максимальная допустимая позиция.
homing_speed:50 # Скорость парковки.
homing_retract_dist:0 # Расстояние отката после парковки для повторной парковки, при нуле нет второй парковки
homing_positive_dir:False # Направление парковки (False = к 0).
step_pulse_duration:0.0000001 # Длительность импульса шага.

# Настройки шагового двигателя оси Y
[stepper_y]
step_pin:U_1:PC14 # Пин для шагов.
dir_pin:!U_1:PC13 # Пин для направления (инвертирован).
enable_pin:!U_1:PC15 # Пин для включения (инвертирован).
microsteps: 16 # Количество микрошагов.
rotation_distance: 39.88 # Расстояние (в мм) за один оборот мотора.
full_steps_per_rotation:200 # Количество полных шагов на оборот.
endstop_pin:tmc2240_stepper_y:virtual_endstop # Пин концевика (виртуальный).
position_min: -4.5 # Минимальная допустимая позиция.
position_endstop: -4.5 # Позиция концевика.
position_max: 258 # Максимальная допустимая позиция.
homing_speed:50 # Скорость парковки.
homing_retract_dist:0 # Расстояние отката после парковки для повторной парковки, при нуле нет второй парковки
homing_positive_dir:False # Направление парковки (False = к 0).
step_pulse_duration:0.0000001 # Длительность импульса шага.

# Настройки шагового двигателя оси Z
[stepper_z]
step_pin:U_1:PC10 # Пин для шагов.
dir_pin:U_1:PA15 # Пин для направления.
enable_pin:!U_1:PC11 # Пин для включения (инвертирован).
microsteps: 128 # Количество микрошагов.
rotation_distance: 4 # Расстояние (в мм) за один оборот мотора.
full_steps_per_rotation: 200 # Количество полных шагов на оборот.
endstop_pin:probe:z_virtual_endstop # Пин концевика (виртуальный, на основе зонда).
endstop_pin_reverse:tmc2209_stepper_z:virtual_endstop # Виртуальный концевик для обратной парковки.
position_endstop:-0.2 # Позиция концевика.
position_endstop_reverse:248 # Позиция обратного концевика.
position_max:248 # Максимальная допустимая позиция.
position_min: -4 # Минимальная допустимая позиция.
homing_speed: 8 # Скорость парковки.
homing_speed_reverse: 8 # Скорость обратной парковки.
second_homing_speed: 10 # Вторая скорость парковки.
homing_retract_dist: 5.0 # Расстояние отката после парковки.
homing_positive_dir:false # Направление парковки (False = к 0).
homing_positive_dir_reverse:true # Направление обратной парковки (True = от 0).
step_pulse_duration:0.0000001 # Длительность импульса шага.

# Настройки второго шагового двигателя оси Z
[stepper_z1]
step_pin:U_1:PB1 # Пин для шагов.
dir_pin:U_1:PB6 # Пин для направления.
enable_pin:!U_1:PB0 # Пин для включения (инвертирован).
microsteps: 128 # Количество микрошагов.
rotation_distance: 4 # Расстояние (в мм) за один оборот мотора.
full_steps_per_rotation: 200 # Количество полных шагов на оборот.
step_pulse_duration:0.0000001 # Длительность импульса шага.
endstop_pin_reverse:tmc2209_stepper_z1:virtual_endstop # Виртуальный концевик для обратной парковки.

# Компенсация наклона оси Z
[z_tilt]
z_positions:
    -59,125 # Координаты первой точки, к которой прикреплена ось Z.
    307.5,125 # Координаты второй точки, к которой прикреплена ось Z.
points:
    0,125 # Координаты первой точки, которую нужно измерить.
    215,125 # Координаты второй точки, которую нужно измерить.
speed: 150 # Скорость движения во время калибровки.
horizontal_move_z: 5 # Высота (в мм) для горизонтальных перемещений.
retries: 2 # Количество повторных попыток, если точки не в пределах допуска.
retry_tolerance: 0.05 # Допустимая разница между результатами измерений.

# Конфигурация драйвера TMC2240 для оси Y через SPI
[tmc2240 stepper_y]
cs_pin:U_1:PB9 # Пин выбора чипа.
spi_software_sclk_pin:U_1:PA5 # Пин тактового сигнала SPI.
spi_software_mosi_pin:U_1:PA7 # Пин для передачи данных SPI.
spi_software_miso_pin:U_1:PA6 # Пин для получения данных SPI.
spi_speed:200000 # Скорость SPI.
run_current: 1.07 # Рабочий ток (в амперах RMS).
interpolate:true # Включение интерполяции микрошагов.
stealthchop_threshold:0 # Скорость для включения StealthChop. 0 - выключен.
diag0_pin:!U_1:PC0 # Пин для диагностики (концевик без датчика).
driver_SGT:1 # Порог для StallGuard (чувствительность концевика без датчика).

# Конфигурация драйвера TMC2240 для оси X через SPI
[tmc2240 stepper_x]
cs_pin:U_1:PD2 # Пин выбора чипа.
spi_software_sclk_pin:U_1:PA5 # Пин тактового сигнала SPI.
spi_software_mosi_pin:U_1:PA7 # Пин для передачи данных SPI.
spi_software_miso_pin:U_1:PA6 # Пин для получения данных SPI.
spi_speed:200000 # Скорость SPI.
run_current: 1.07 # Рабочий ток (в амперах RMS).
interpolate:true # Включение интерполяции микрошагов.
stealthchop_threshold:0 # Скорость для включения StealthChop.
diag0_pin:!U_1:PB8 # Пин для диагностики (концевик без датчика).
driver_SGT:1 # Порог для StallGuard.

# Конфигурация драйвера TMC2209 для оси Z через UART
[tmc2209 stepper_z]
uart_pin:U_1: PC5 # Пин для UART-связи.
run_current: 0.6 # Рабочий ток (в амперах RMS).
interpolate: True # Включение интерполяции.
stealthchop_threshold: 9999999999 # Отключение StealthChop (установка очень большого значения).
diag_pin:^U_1:PC12 # Пин для диагностики (концевик без датчика).
driver_SGTHRS:130 # Порог для StallGuard (установлен на 130).

# Конфигурация драйвера TMC2209 для оси Z1 через UART
[tmc2209 stepper_z1]
uart_pin:U_1: PB7 # Пин для UART-связи.
run_current: 0.6 # Рабочий ток (в амперах RMS).
interpolate: True # Включение интерполяции.
stealthchop_threshold: 9999999999 # Отключение StealthChop.
diag_pin:^U_1:PA13 # Пин для диагностики (концевик без датчика).
driver_SGTHRS:130 # Порог для StallGuard (установлен на 130).

# Настройки нагревателя стола
[heater_bed]
heater_pin: U_1:PB10 # Пин, управляющий нагревателем.
sensor_type:NTC 100K MGB18-104F39050L32 # Тип температурного датчика.
sensor_pin:U_1: PA0 # Пин, подключенный к сенсору.
max_power: 1.0 # Максимальная мощность.
control = pid # Алгоритм управления.
pid_Kp=63.418 # Пропорциональный коэффициент PID.
pid_Ki=1.342 # Интегральный коэффициент PID.
pid_Kd=749.125 # Дифференциальный коэффициент PID.
min_temp: -60 # Минимальная безопасная температура.
max_temp: 125 # Максимальная безопасная температура.

# Настройки нагревателя камеры
[heater_generic chamber]
heater_pin:U_1:PC8 # Пин, управляющий нагревателем.
max_power:1.0 # Максимальная мощность.
sensor_type:NTC 100K MGB18-104F39050L32 # Тип температурного датчика.
sensor_pin:U_1:PA1 # Пин, подключенный к сенсору.
control = pid # Алгоритм управления.
pid_Kp=63.418 # Пропорциональный коэффициент PID.
pid_Ki=1.342 # Интегральный коэффициент PID.
pid_Kd=749.125 # Дифференциальный коэффициент PID.
min_temp:-100 # Минимальная безопасная температура.
max_temp:65 # Максимальная безопасная температура.

# Проверка работоспособности нагревателя камеры
[verify_heater chamber]
max_error: 300 # Максимальная кумулятивная ошибка температуры.
check_gain_time:480 # Время проверки нагрева.
hysteresis: 5 # Гистерезис.
heating_gain: 1 # Усиление нагрева.

# Проверка работоспособности нагревателя экструдера
[verify_heater extruder]
max_error: 120 # Максимальная кумулятивная ошибка температуры.
check_gain_time:20 # Время проверки нагрева.
hysteresis: 5 # Гистерезис.
heating_gain: 1 # Усиление нагрева.

# Проверка работоспособности нагревателя стола
[verify_heater heater_bed]
max_error: 200 # Максимальная кумулятивная ошибка температуры.
check_gain_time:60 # Время проверки нагрева.
hysteresis: 5 # Гистерезис.
heating_gain: 1 # Усиление нагрева.

# Вентилятор для дополнительного охлаждения (на задней стенке)
[fan_generic auxiliary_cooling_fan]
pin: U_1:PA8 # Выходной пин для управления вентилятором.
shutdown_speed: 0.0 # Скорость при выключении (0-1).
cycle_time: 0.0100 # Время одного цикла ШИМ.
hardware_pwm: false # Использовать аппаратный ШИМ.
kick_start_time: 0.100 # Время "толчка" для запуска.
off_below: 0.0 # Минимальная скорость для работы.

# Вентилятор для циркуляции воздуха в камере (на боковой стенке)
[fan_generic chamber_circulation_fan] 
pin:U_1:PC9 # Выходной пин для управления вентилятором.
shutdown_speed: 0.0 # Скорость при выключении.
cycle_time: 0.100 # Время одного цикла ШИМ.
hardware_pwm: false # Использовать аппаратный ШИМ.
kick_start_time: 0.100 # Время "толчка".
off_below: 0.0 # Минимальная скорость для работы.

# Вентилятор камеры, управляемый температурой (на грелке)
[chamber_fan chamber_fan]
pin:U_1:PA4 # Пин для вентилятора.
max_power: 1.0 # Максимальная мощность.
shutdown_speed: 0 # Скорость при выключении.
kick_start_time: 0.5 # Время "толчка".
heater:chamber # Связь с нагревателем камеры.
fan_speed: 1.0 # Скорость вентилятора, когда нагреватель включен.
off_below: 0 # Отключение, если скорость ниже 0.
idle_timeout:60 # Время ожидания (в секундах) перед выключением.
idle_speed:1.0 # Скорость в режиме ожидания.

# Вентилятор охлаждения хотэнда (первый)
[heater_fan hotend_fan]
pin:gpio25 # Пин для вентилятора.
max_power: 1.0 # Максимальная мощность.
shutdown_speed:1.0 # Скорость при выключении.
kick_start_time: 0.5 # Время "толчка".
heater: extruder # Связь с нагревателем экструдера.
heater_temp: 50.0 # Температура, при которой включается вентилятор.
fan_speed: 1.0 # Скорость вентилятора.
off_below: 0 # Отключение, если скорость ниже 0.

# Вентилятор охлаждения хотэнда (второй)
[heater_fan hotend_fan2]
pin:gpio11 # Пин для вентилятора.
max_power: 1.0 # Максимальная мощность.
shutdown_speed:1.0 # Скорость при выключении.
kick_start_time: 0.5 # Время "толчка".
heater: extruder # Связь с нагревателем экструдера.
heater_temp: 50.0 # Температура, при которой включается вентилятор.
fan_speed: 1.0 # Скорость вентилятора.
off_below: 0 # Отключение, если скорость ниже 0.

# Вентилятор охлаждения платы
[controller_fan board_fan]
pin:U_1:PC4 # Пин для вентилятора.
max_power:1.0 # Максимальная мощность.
shutdown_speed:1.0 # Скорость при выключении.
cycle_time:0.01 # Время цикла ШИМ.
fan_speed: 0.6 # Скорость вентилятора.
stepper:stepper_x,stepper_y # Включается при активности указанных моторов.

# Вентилятор обдува модели
[fan_generic cooling_fan]
pin:gpio2 # Выходной пин для управления вентилятором.
max_power: 1.0 # Максимальная мощность.
shutdown_speed: 0 # Скорость при выключении.
cycle_time: 0.0100 # Время цикла ШИМ.
hardware_pwm: false # Использовать аппаратный ШИМ.
kick_start_time: 0.100 # Время "толчка".
off_below: 0.0 # Минимальная скорость для работы.

# Настройка выходного пина для подсветки
[output_pin caselight]
pin: U_1:PC7 # Пин.
pwm: false # Отключен ШИМ.
shutdown_value:1 # Значение пина при выключении.
value:1 # Начальное значение пина.

# Настройка выходного пина для бипера
[output_pin beeper]
pin:U_1: PA2 # Пин.
pwm: false # Отключен ШИМ.
shutdown_value:0 # Значение пина при выключении.
value:0 # Начальное значение пина.

# Настройка выходного пина для светодиода состояния
[output_pin ctlyd]
pin:U_1: PA14 # Пин.
pwm: false # Отключен ШИМ.
shutdown_value:0 # Значение пина при выключении.
value:0 # Начальное значение пина.

# Настройки тенз под столом
[smart_effector]
pin:U_1:PC1 # Пин, подключенный к выходу зонда.
recovery_time:0 # Задержка между перемещением и началом измерения.
x_offset: 17.6 # Смещение по оси X.
y_offset: 4.4 # Смещение по оси Y.
z_offset: 0.000001 # Смещение по оси Z.
speed:5 # Скорость Z-оси при измерении.
lift_speed:5 # Скорость подъема между измерениями.
probe_accel:50 # Ускорение при измерении.
samples: 2 # Количество измерений в каждой точке.
samples_result: submaxmin # Метод обработки результатов (удаление максимального и минимального).
sample_retract_dist: 5.0 # Расстояние отвода после измерения.
samples_tolerance: 0.05 # Допустимое отклонение между измерениями.
samples_tolerance_retries:5 # Количество повторных попыток при выходе за пределы допуска.

# Настройки для "qdprobe", механизм чиди для переключения работы индукция/тензы
[qdprobe]
pin:!gpio21 # Пин (инвертирован).
z_offset:0.000001 # Смещение по оси Z.

# Настройки для автоматической калибровки сетки стола (bed mesh)
[bed_mesh]
speed:150 # Скорость перемещения.
horizontal_move_z:7 # Высота для горизонтальных перемещений.
mesh_min:20,15 # Минимальные X, Y координаты сетки.
mesh_max:230,230 # Максимальные X, Y координаты сетки.
probe_count:8,8 # Количество точек для калибровки по каждой оси.
algorithm:bicubic # Алгоритм интерполяции.
bicubic_tension:0.2 # Коэффициент натяжения для бикубической интерполяции.
mesh_pps: 2, 2 # Количество точек на сегмент для интерполяции.
vibrate_gcode:
    Z_DOUDONG # Макрос для "пробуждения" тенз.

# Датчик запутывания филамента
[filament_switch_sensor fila]
pause_on_runout: True # Пауза при обнаружении отсутствия филамента.
runout_gcode:
    M118 Filament tangle detected # G-код, выполняемый при обнаружении проблемы.
event_delay: 3.0 # Задержка перед срабатыванием.
pause_delay: 0.5 # Задержка перед паузой.
switch_pin:U_1:PC3 # Пин, к которому подключен переключатель.

# Настройки теста резонансов
[resonance_tester]
accel_chip:adxl345 # Имя акселерометра для измерений.
probe_points:
    120, 120, 10 # Координаты точки для теста.

# Макрос для отмены печати
[gcode_macro_break]
# Используется для отмены печати в макросе.

# Таймаут бездействия
[idle_timeout]
timeout: 43200 # Время бездействия (в секундах) перед выполнением G-кода.
gcode:
    PRINT_END # G-код, выполняемый по истечении таймаута.

# Настройки для паузы/возобновления печати
[pause_resume]

# Отображение статуса
[display_status]

# Виртуальная SD-карта
[virtual_sdcard]
path: ~/gcode_files # Путь к директории с G-кодом.
```
