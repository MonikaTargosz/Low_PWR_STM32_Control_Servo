## Microcontroller

- STM32 Nucleo-F411RE high performance.
- On pins PA6, PA7 and PB0 there is LED1, LED2, LED3 which are off in high state and on in low state. 

## Assumption

We are generating a PWM signal which is useful to control the brightness of the LED. 

### Configuration
- We use a general purpose TIM-3 16-bit counter connected to the APB2 bus. We choose Internal Clock as the clock source.
- Set the pin functions to, respectively: TIM3_CH1, TIM3_CH2, and TIM3_CH3. We name them LED1, LED2 and LED3.- We want a frequency of 100 Hz by setting Prescaler to 79 and the value for Counter Period to 9999.
- In CubeMX, configure TIM3 and set the channels to, respectively: PWM Generation CH1, PWM Generation CH2, and PWM Generation CH3. 

### Software: 

- We use the interrupt reported when the main timer counter overflows, that is, the HAL_TIM_PeriodElapsedCallback function to change the state of LED2.
- The HAL_TIM_OC_Start_IT function is used to start the diode on the Nucleo board. 
- The HAL_TIM_PWM_Start function is used for the other external diodes, and bypassing "_IT" will ensure that interrupts are not reported.

## Realisation
We will not see the LEDs blinking, but we will see that they glow with different brightness.
 
The dependence of the LEDs' brightness on the PWM filling is not linear. In order to get the effect like in the picture you need to set Pulse values, which are related to successive channels: 50, 500 and 5000.

To get the effect of blinking leds was easy to verify with the "naked eye" the frequency of the timer should be low around 1 Hz. 

