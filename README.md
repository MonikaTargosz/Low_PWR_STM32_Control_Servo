## Microcontroller

- STM32 Nucleo-F411RE high performance.

![image](https://user-images.githubusercontent.com/37025393/127035113-962d626d-cf59-430f-8785-a272a55102f6.png)

## Assumption

After the servo has completed its task, the system enters stop mode to reduce power consumption.

### Configuration

- On pin PA5 there is LD2 (LED) which are off in high state and on in low state. 
- On pin PC13 there is GPIO Mode as external interrupt.
- On PA0 there is sensor i.e. a servo actuator.

### Software: 

- The security setting that if we are not in the stop to it we go into it:
```
if(StopModeFlag == 0)
	  {
		  StopModeFlag = 1;
		  HAL_PWR_EnterSTOPMode(PWR_LOWPOWERREGULATOR_ON, PWR_SLEEPENTRY_WFI);
	  }
```
- HAL_GPIO_EXTI_Callback() - Handling the interrupt involves reconfiguring the clocks after the microcontroller is up and the tasks in the while loop are completed.
- Function for setting the servo to a preset position:
 - Servo_SetAngle() - by the given integer angle,
 - Servo_SetAngleFine() - by the given "fine" fractional angle.

## Realisation
To reduce power consumption in stop mode, in addition to disabling the core, we disable the fast oscillators as well as the peripherals that are powered by those oscillators, so they will not work. The interrupts from the peripherals will not work. 

The regulator and slow oscillators such as RTC, watchdog will work.

Waking up the microcontroller is possible via external interrupt or wakeup event. For this purpose, the B1 as EXTI13 button was used to wake up the microcontroller from the STOP state.

The internal RAM along with the registers is preserved. We go down in power consumption, but we do not lose the program. 

![image](https://user-images.githubusercontent.com/37025393/127035561-c4081229-06e0-4482-a321-0931c63a1ebe.png)


