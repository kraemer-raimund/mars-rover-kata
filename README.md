# Mars Rover Kata

My take at a popular coding kata that already exists in many variants by different authors.

## Intro

You are part of the Mars Rover mission, creating the software that accepts and interprets commands to steer the rover.

## 1. Evolutionary TDD Kata

Learning goals: evolutionary design, systems thinking

*Make sure to reveal the requirements of the next version only after finishing the previous one.*

### Version 1 Requirements

<details>
<summary>Reveal</summary>

- The Mars rover has a position (x, y) starting at (0, 0), i.e., every positional change is relative to its starting point. It moves only in increments of exactly 1 meter.
- It also has a direction (N, S, W, E), initially facing north (N) and rotating in increments of exactly 90 degrees.
- The Mars rover accepts a sequence of movement commands, executes them in order of occurrence, and returns the resulting position and direction in the format, e.g., "x:2, y:1, d:E".
- The 3 supported commands are M (move forward), L (turn left), and R (turn right). They can be chained into a command sequence, e.g., "MMLMRM".

</details>

### Version 2 Requirements

<details>
<summary>Reveal</summary>

- As a Mars rover pilot, I want to write long command sequences more efficiently. E.g., "11MLR" shall be interpreted the same way as "MMMMMMMMMMMLR".
- As a Mars rover pilot, I want to receive info about the battery level of the Mars rover after each command sequence. The battery level shall be part of the output, e.g., "x:2, y:1, d:E, b:85". The Mars rover starts at a battery level of 100, and loses exactly 1 percent per movement command (moving 1 meter, or rotating 90 degrees left/right).
- As a Mars rover engineer, I want to save precious battery capacity by optimizing inefficient rotation sequences. "LLL" shall result in a single rotation right. "LLLL" shall be ignored, since it would just turn the rover in place, wasting battery while resulting in the same direction.

</details>

### Version 3 Requirements

<details>
<summary>Reveal</summary>

**Background info:** When the battery level reaches 0, the hardware automatically shuts down and waits for the battery to be fully recharged via the rover's solar panels. There is nothing we can do about this on the software side. Unfortunately, the rover also completely resets its state after rebooting.

- As a Mars rover pilot, I want to be able to manually reinitialize the rover to its previous state (i.e., position and direction), since the rover itself does not persist that state after a reboot. When the rover's battery is depleted, before shutting down it shall print its current state as well as the remaining command sequence that it wasn't able to finish, e.g., "x:2, y:1, d:E, b:0, MMLM2RM". After a reboot, it shall ignore all movement commands/command sequences until it received an initialization command in the form, e.g., "(2, 1, E)", after which it is again ready to accept movement commands. Receiving a movement command before the initialization command shall result in an error message, transmitted via the same channel as the status after a command sequence.
- As a Mars rover pilot, I want to know in advance whether a command sequence can be completed with the current battery capacity. When the rover receives a command sequence that ends in a "?" (e.g., "M2LMR3M?"), it shall answer with the current battery capacity and the remaining battery capacity after the command sequence, and then immediately prompt the pilot whether it should now execute that command sequence, accepting "y" (yes) or "n" (no).

</details>

### Version 4 Requirements

<details>
<summary>Reveal</summary>

**Background info:** The rover now has an integrated flash drive and persists its state when the battery is depleted!

- In case the rover was not able to finish a movement command sequence before shutting down, then after a reboot and successful execution of the initialization command the rover shall prompt the pilot whether it should finish the last command sequence (via either "y" or "n").
- To further save battery capacity, we now allow for diagonal movements:
  - Support directions "NE", "SW", etc., resulting in 8 possible directions. For "small" rotations of 45 degrees, we use the "l" and "r" commands, while keeping "L" and "R" for 90 degree rotations. Small rotations use up 0.5 percent battery capacity instead of 1 percent.
  - When moving, stick to the grid, i.e. diagonal movements are slightly longer than 1 meter, while axis-aligned movements stay at 1 meter per increment. Note: This also increases the battery capacity used for diagonal movement commands, which shall be displayed in the status message with a precision of 2 decimal places (rounding down), e.g., "x:2, y:1, d:E, b:85.42".
  - If the current capacity is not sufficient for the next command, the rover shall shut down and start recharging immediately; i.e., in some cases, the rover might have enough battery capacity for a horizontal movement but not for a diagonal movement, or enough for a 45 degree rotation but not for a 90 degree rotation, etc.

</details>

___

© 2023 Raimund Krämer - Use with attribution.

Links to third party sites are included for convenience only and I am not responsible for their contents.
