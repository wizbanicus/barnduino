# barnduino

Arduino project for barn. The purpose is to enable more use of available sunlight without adding batteries and without basing decisions on time of day. Also aims at enabling users to use power without having to think too much (just general conservation e.g. turn off what your aren't using) as the parameters would be set for them by the system (based on their prefered needs - see table for this). First up we set the rules, define what we want to measure, and set the power scenarios. I'll put them in a table then we can make some code to handle each scenario. 

## measurables

- Freezer Temp (very cold, cold, warm)
- Usable Excess Power Incoming (Y/N)
- Battery level (Off, Very Low, Low, Medium, High)

## states

**These are up for debate**
- (D) dead, safety has failed batteries dead 
- (PwD) powered down, all power is off including arduino & inverter, manual restart required (PwD)
- (A) aux only, auxiliary strip lighting only
- (F) freezer
- (L) lights
- (W) wall sockets for electronics (users need to know not to use heat or motor devices such as toasters heaters hair dryers etc)
- (P) pumps, for shower
- (B) extra batteries, eg for powertools

## Table

**These are up for debate**
It suggests that the most important thing is Aux lighting, second is keepiong food cold, third is lighting and electronics, fourth is shower, fourth is keeping freezer very cold, fifth is extra batter storage 
| **battery** |          |            |            |        |          |            |
| ---         | ---      | ---        | ---        | ---    | ---      | ---        |
| high        | A+F+L+W  | A+F+L+W+P  | A+L+W+P+B  |   A+F  |  A+L+W+P |   A+L+W+P  |
| meduim      | A+F+L+W  | A+F+L+W+P  | A+L+W+P+B  |   A+F  |  A+L+W   |   A+L+W    |
| low         | A+F      | A+L+W      | A+L+W      |   A    |  A       |   A        |
| very low    | PwD      | PwD        | PwD        |   PwD  |  PwD     |   PwD      |
| off         | D        | D          | D          |   D    |  D       |   D        |
| **freezer** | warm     | cold       | very cold  | warm   | cold     | very cold  |      
| **excess**  | yes      | yes        | yes        | no     | no       | no         |

## Excess power when freezer is running

In this version we are not accounting for a scenario where the freezer is running and there is still enough excess power to do more. That would make a great V2.

## possible loop and functions

**main loop**
- wait 60 seconds
- check state (returns 3 char code [H,M,L,V][W,C,V][Y,N])
- switch based on state - turn on and off the correct circuits

**functions**
- getBatteryReading(returns char [H,M,L,V])
- getFreezerTemp(returns char [W,C,V])
- getExcessState(returns char [Y,N])
- aux([on,off])
- freezer([on,off])
- lights([on,off])
- wallSockets([on,off])
- pumps([on,off])
- extraBatteries([on,off])
- powerDown (switches off everything including the inverter and arduino itself)

