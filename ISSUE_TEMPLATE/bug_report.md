# Intermittent Wheel Control Issue: Single Wheel Spinning at Full Speed

## Issue Type
- [x] Bug
- [ ] Feature Request
- [ ] Documentation
- [ ] Question

## Repository
`dp_ros2_control`, `dp_embedded_fw`

## Date Occurred
December 17, 2025

## Severity
- [ ] Critical (Safety Hazard)
- [x] High (Robot Malfunction)
- [ ] Medium (Degraded Performance)
- [ ] Low (Minor Issue)

## Description

When taking control of the robot using the tethered controller after power-up, one wheel spins at full speed while the other wheel remains stationary, causing the robot to behave erratically.

## Steps to Reproduce

1. Turn on main power
2. Flip the tethered controller's switch to forward position to take over control
3. Push the joystick forward slightly (or any direction)
4. **Observed**: One wheel spins at full speed while the other wheel does nothing
5. **Expected**: Both wheels should respond proportionally to joystick input

## Frequency
- **Occurrence**: Very rare (only 2 times in the past year)
- **Intermittent**: Yes
- **Reproducible**: No (has not been consistently reproducible)

## Environment

- **Hardware**: David's Robot (telehealth robot)
- **Battery Level**: Low (approximately 20% or below)
- **Power State**: Immediately after main power-on

## Affected Components

- **Motor Controllers**: CAN bus motor controllers (one wheel affected)
- **Wheels**: Physical wheel assembly
- **Electronics Board**: Main control board handling CAN communication
- **Battery**: Battery Management System (BMS)
- **Tethered Controller**: Physical controller with joystick input

**Note**: This issue occurs even without powering the Jetson Orin, suggesting it may be related to low-level hardware/firmware rather than high-level software components.

## Technical Details

### Potential Root Causes

1. **Battery-Related Issue**: 
   - Last occurrence: Battery was too low causing BMS to shut down the system. After charging and restarting, this issue occurred.
   - Current occurrence: Battery level was around 20% (already too low) and robot had been stored in cold conditions.
   - Low battery voltage may cause unstable power delivery to motor controllers, leading to initialization or communication issues.

2. **CAN Bus Communication Issue**: Race condition or message corruption during power-up initialization, especially under low voltage conditions.

3. **Motor Controller Initialization**: One motor controller not properly initialized when first command is sent, possibly due to insufficient power or timing issues during startup.

4. **Electronics Board Power Sequencing**: Incorrect power sequencing or insufficient power delivery to motor controllers during startup, exacerbated by low battery conditions.

5. **Temperature Effects**: Cold storage may affect battery performance and power delivery characteristics, contributing to initialization issues.

6. **Threading/Race Condition**: CAN mutex or command processing timing issue, potentially worsened by unstable power conditions.

## Logs/Diagnostics

(To be filled in when available)

## Emergency Response (If Issue Occurs)

If this issue happens:

1. **Option 1 - Power Shutdown**: Shutdown the main power immediately. **Note**: If the robot is on the ground, it will rotate/spin, making this difficult to execute safely.

2. **Option 2 - STO via Tethered Controller** (Recommended): Use the tethered joystick to move the switch all the way back to **OFF mode**. This will **activate STO (Safe Torque Off)** on the motor controller, which stops the motors by disabling torque generation.

## Workaround

**CAUTION**: Before starting the robot, consider placing it on a stand or elevated surface so wheels do not touch the ground. This prevents unexpected movement if the issue occurs. If everything appears normal after power-up and initial testing, operate normally.

## Related Issues
None (first occurrence being documented)

## Labels
- `bug`
- `hardware-interface`
- `motor-control`
- `intermittent`
- `high-priority`
- `battery-related`
