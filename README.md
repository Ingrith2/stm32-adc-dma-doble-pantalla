Monitor de dos señales analógicas con STM32F411 (ADC + DMA) y doble pantalla
Embedded firmware (C, STM32 HAL): 2-channel ADC with DMA, shown on an SSD1306 OLED and a 1.28" round LCD.
Descripción
Firmware para una tarjeta STM32F411CEU6 que lee dos señales analógicas (0 – 3.3 V), las convierte a voltaje y las muestra en tiempo real en dos pantallas distintas:
OLED SSD1306 (SPI1): muestra el voltaje del canal 1.
LCD circular de 1.28" (240×240, Waveshare) (SPI2): muestra el voltaje del canal 2 dentro de una interfaz con marco circular.
Características técnicas
Elemento	Detalle
Microcontrolador	STM32F411CEU6 (Cortex-M4)
Reloj	HSI 16 MHz → PLL (M=8, N=72, P=2) → SYSCLK 72 MHz, HCLK 18 MHz
ADC	ADC1, canales IN8 (PB0) e IN9 (PB1), 12 bits, modo continuo con escaneo, tiempo de muestreo de 480 ciclos
Transferencia	DMA en modo continuo hacia un arreglo en RAM
Conversión	`V = (valor_ADC × 3.3) / 4095`
Pantalla 1	SSD1306 por SPI1 (PA5 SCK, PA7 MOSI; CS PA0, DC PA1, RES PA2)
Pantalla 2	LCD redondo 1.28" por SPI2 (PB10 SCK, PB15 MOSI; CS PB12, DC PB13, RES PA8)
Depuración	USART1 a 115200 baudios (PA9 TX, PA10 RX) y SWD
Indicador	LED de la tarjeta (PC13) que parpadea en cada ciclo de lectura
Cómo funciona
Se inicializan reloj, GPIO, DMA, SPI1, SPI2, USART1 y ADC1.
Se inician las dos pantallas y se dibuja el marco circular en el LCD.
El ADC convierte los dos canales de forma continua y el DMA deja los resultados en `VALUE[0]` y `VALUE[1]`.
En el ciclo principal se convierte cada valor a voltios y se escribe en su pantalla.
Cómo compilarlo
Instala STM32CubeIDE.
Clona este repositorio e impórtalo: File → Import → Existing Projects into Workspace.
Abre el archivo `.ioc` y pulsa Generate Code para que CubeIDE descargue la carpeta `Drivers/` (HAL y CMSIS).
Compila y carga en la tarjeta con un ST-Link.
Créditos y librerías de terceros
Librería SSD1306 de Olivier Van den Eede (4ilo) y Aleksander Alekseev (afiskon): https://github.com/afiskon/stm32-ssd1306
Controlador del LCD redondo y librería gráfica (`LCD_1inch28`, `GUI_Paint`, `DEV_Config`, fuentes): código de ejemplo de Waveshare.
`UartRingbuffer`: Controllerstech.
HAL y CMSIS: STMicroelectronics.
Autora
Ingrith Natalia Villegas Medina: Ingeniera Electrónica y Telecomunicaciones
