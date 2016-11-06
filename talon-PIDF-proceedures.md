#Talon Closed Loop PIDF tuning Tips and Tricks

### References

Much of this information comes from the [Talon SRX Software Reference Manual](http://www.ctr-electronics.com/talon-srx.html#product_tabs_technical_resources)

## Java Motion Profile Loop - position

```
double p = 0.1;       /*Kp */
double i = 0.001;     /*Ki */
double d = 1;         /*Kd */
double f = 0.0001;    /*Kf FeedForward*/
int izone = 300;      /* encoder ticks / analog units clearing Integral accumulator (see note:1)*/
double ramprate = 36; /* volts per second */
int profile = 0;      /* can be 0 or 1 */
customMotorDescript.setPID(p, i, d, f, izone, ramprate, profile);
```
**Notes:**

1. Clearing the Integral accumulator is neccesary to prevent integral windup. 
  - If using izone this is done automatically when the closed loop error is outside the izone value.
  - Manually clearing the Integral accumulator with Java. customMotorDescrip.ClearIaccum();
  - Whenever the control mode of the Talon is changed.
  - When a Talon is in the disabled state.
  - When the motor control profile slot has changed.
  - When the Closed loop Error's magnitude is smaller then the "Allowable Closed Loop Error".
2. The Talon SRX can have two PIDF profiles that can be selected at any time. 0 and 1 set for profile.
3. The CTRE Mag encoder is a 4x encoder with rise and fall values.  So one rotation is 4096 units per rotation.  

## Steps for tuning the PID loop
Examples to run the tests below can be found on the [CTR GitHub Account](https://github.com/CrossTheRoadElec/FRC-Examples)

1. It is important to make sure the Talon and Encoders are meshed properly.  Both motor and encoder need to both going in a posotive direction.
To test this, run the motor in forward (green throttle lights) and make sure the encoder values are also moveing in a potitive direction.
1. Set all PIDF values to 0.
1. Run the profile while reporting the current output of the profile, the speed getSpeed(), the error between the two and _talon.GetClosedLoopError.
1. Set the Kf to get realtime results closer.
1. Then set the Kp to get the final potion close.
1. Set the Kd to smooth out any sudden/violent motions.  Start with 10x the Kp gain.
1. If the mechanism is not quite reaching the target and the Kp gain cannot be increased any further without hurting overall performance, then begin adding Ki gain. Start with 1/100 of the Kp gain.

