# separator_control
Simulink study of PI level control, disturbance rejection and valve saturation in a simplified separator vessel..



# Simulink PI Level Control Model for an Oil and Gas Separator

A quick educational MATLAB and Simulink model looking at how different PI controller tunings handle liquid level control in a simplified separator vessel especially when a sudden slug of liquid comes through

## What this model does

The project sets up a basic mass balance for a separator vessel

$$\frac{dh}{dt} = \frac{Q_{\mathrm{in}} - Q_{\mathrm{out}}}{A}$$

Where $h$ is liquid level $Q_{\mathrm{in}}$ and $Q_{\mathrm{out}}$ are inflow and outflow rates and $A$ is the vessel cross sectional area

A PI controller looks at the error between the actual level and the setpoint then adjusts the outlet valve The model also accounts for valve saturation limits and includes some basic high and low alarm and trip logic

We run a 3000 second simulation to see how the system reacts to a sudden step increase or slug in liquid inflow comparing two different control strategies

1 Tight Level Control Prioritizes keeping the liquid level glued right to the setpoint
2 Averaging Level Control Lets the level bounce around a bit more which saves the outlet valve from working overtime and keeps downstream flow steadier

## Controller Settings

| Controller | Proportional Gain $K_p$ | Integral Gain $K_i$ |
| --- | --- | --- |
| Tight | 5.0 | 0.01 |
| Averaging | 0.5 | 0.001 |

These are just demo parameters for the simulation not tuned for a specific physical plant

## What we found

Under a standard slug disturbance

* Tight tuning kept the peak level down to 1.066 m but resulted in a sharp aggressive swing in outlet flow
* Averaging tuning let the level rise a bit higher to 1.243 m but smoothed out the outlet flow significantly by using the vessel inventory as a buffer

When we cranked up the disturbance size to test limits

* The tight controller maxed out the valve actuator limits
* The averaging controller tripped the high level alarm threshold due to the larger inventory swing

Check out the results folder to see screenshots of the model layout and response plots

## Takeaways for Process Control

It is a classic trade off where tightly regulating a level is not always the best engineering choice In a separator letting the liquid level flex a little bit can act as a useful surge buffer If your control valve reacts too aggressively you just pass those flow disturbances straight down the line to the next unit Ultimately tuning depends on downstream sensitivity valve wear and tear and how big your expected slugs actually are

This is a simplified educational model not a rigorous engineering study It does not factor in actual sizing standards multiphase hydrodynamics pressure control interactions or a certified Safety Instrumented System

## Tools Used

* MATLAB and Simulink
* Process dynamics and inventory mass balancing
* Disturbance response analysis
* Actuator saturation and threshold logic

## How to run it

1 Open separator.slx in MATLAB and Simulink
2 Poke around the subsystem blocks to see how the vessel balance PI loops and alarm logic are wired up
3 Hit run and check out the scopes to compare how the tight versus averaging tunings behave
4 Tweak the disturbance magnitude to see where the valve saturates or the alarms trigger
