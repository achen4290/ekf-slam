# EKF-SLAM
Python package for EKF-SLAM (simultaneous localization and mapping with the extended Kalman filter). Uses known correspondence where once a landmark is observed, the agent knows which landmark the observation belongs to.

Given data collected from a agent with a noisy sensor and noisy movement controls, this EKF-SLAM program can predict the position of the agent with a movement vector as input, using a provided estimated movement noise model, and update the agent's and landmark position's with list of noisy observations, using a provided estimated sensor noise model.

## Installation
`pip install ekf-slam`

## Development
1. Clone this repo
2. Create a new python environment
3. `pip install flit`
4. `flit install -s`

Follow this [README for developers](https://github.com/achen4290/ekf-slam/blob/main/dev/README.md) for more information. 

## Example Usage

Function docstrings: 

```python 
KnownCorrespondence(init, num_landmarks, Qt, Rt)
```
```
Initializes calculator for 2D EKF-SLAM with known correspondence
:param init: Array for starting robot position with shape (3,) [x, y, absolute heading in radians]
:param num_landmarks: Number of total landmarks
:param Qt: Measurement noise model (2x2)
:param Rt: Movement nose model (3x3)
```

```python
predict(u_t)
```
```
Prediction step of robot state after recieving movement vector; updates robot position and heading only

u_t[0] = magnitude of movement ()
u_t[1] = angle of movement in radians

:param u_t: Movement vector for the robot (2x1)
```

```python
update(z)
```
```
Update robot and landmark locations based on observations of landmarks

:param z: 2D np array of length K, where K is the number of landmarks observed.
    Each row is a 3D vector [distance, angle, landmark ID]
```

Example of one EKF-SLAM run on noisy data (one movement vector and one list of noisy landmark observations). 

```python
from ekf_slam import KnownCorrespondence
init, num_landmarks, Qt, Rt = ... # inital robot position, number of landmarks, sensor and movement noise parameters 
u_t = ... # movement vector 
z = ... # list of noisy landmark observations
known_correspondence = KnownCorrespondence(init, num_landmarks, Qt, Rt)
known_correspondence.predict(u_t)
known_correspondence.update(z)
```




### Tests 
Example test script in [\tests](https://github.com/achen4290/ekf-slam/tree/main/tests). Provides example parameters for `Qt` and `Rt`, measurement and sensor noise models, although these values should be modified to the sensor and robot that collected the noisy data. 

## Simulation 
We provided a simulation to visualize EKF-SLAM ([README for developers](https://github.com/achen4290/ekf-slam/blob/main/dev/README.md)). 

Estimated robot and landmark states after 1st set of landmark observations. 

<img width="287" alt="image" src="https://github.com/achen4290/ekf-slam/assets/37271059/55dcfef3-1d66-4c03-9ba7-ddb2b6c19719">

Estimated robot and landmark states after 15th set of landmark observations.

<img width="284" alt="image" src="https://github.com/achen4290/ekf-slam/assets/37271059/eaace64e-ec52-4362-a6fb-67e9c618a3c4">
