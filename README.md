# **Self-Driving Car Engineer Nanodegree** 

## Luis Miguel Zapata

---

**PID Control**

This projects aims to develop PID controller to position a simulated autonomous car as close as posible to the middle of a lane.

[image1]: ./screenshots/simulator.png "Simulator"
[image2]: ./screenshots/pid.png "PID"

### 1. Environment.
Simulator is based in Unity and mimics a car for autonomous driving in a single lane road. The vehicle is controlled from a c++ script using web sockets and its velocity will increment constantly untill it reaches 100mph. The task of our controller is to maintain the vehicle within the road commanding the appropiate steering angles using a given cross track error or in other words how far is the car from the lanes center.

![][image1]

### 1. PID controller.
As stated before the PID controller uses the cross track error to calculate steering angle commands, however, it is not only the current CTE (cross track error) that account for aor equations, but we also need its derivative and its integral. In discrete terms a derivative is just the current measurement minus the previous measurement, taht is why we store previous mesurements at every iteration. On the other hand the integral is not more than the summatory of all CTEs.

```
p_error = cte;
i_error += cte;
d_error = cte - prev_cte;
prev_cte = cte;
```

Having defined the proportional, detivative and integral gains (Kp, Kd, Ki) the PID equation will simply be as follows:

```
return -Kp * p_error - Kd * d_error - Ki * i_error; 
```

### 1. PID tuning.
The proportional term in the PID controller is the one that tries to directly compensate for the error observed, however, this direct compensation is not always proportionl to the chosen parameter and in many ocassions compansates more than what is necessary causing overshoots.

The derivative term compensantes for changes in the CTE over time, for instance if the error is constant the derivative does no correction. However in this case the derivative term compensates for the overshoot caused by the proportional term.

Finally the integral term compensates for the stationary state error or bias.

There are many methodologies to chose appropiate PID gains such as twiddle tought in this same course. The methodoly used this time was by trial an error using the following steps:


* Set all gains to zero.
* Increase the P gain until the response to a disturbance is steady oscillation.
* Increase the D gain until the the oscillations go away.
* Repeat steps 2 and 3 until increasing the D gain does not stop the oscillations.
* Set P and D to the last stable values.
* Increase the I gain until it brings you to the setpoint with the number of oscillations desired (normally zero but a quicker response can be had if you don't mind a couple oscillations of overshoot)

The chosen values where:

```
double Kp = 0.25;
double Ki = 0.001;
double Kd = 3.0;
```

![][image2]