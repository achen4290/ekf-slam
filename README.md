# EKF-SLAM

<img width="190" alt="image" src="https://github.com/achen4290/ekf-slam/assets/37271059/55dcfef3-1d66-4c03-9ba7-ddb2b6c19719"> <img width="190" alt="image" src="https://github.com/achen4290/ekf-slam/assets/37271059/eaace64e-ec52-4362-a6fb-67e9c618a3c4"> <img width="186" alt="image" src="https://github.com/achen4290/ekf-slam/assets/37271059/d54b8d7c-f54a-472e-9d0b-7b74f34c1a53"> <img width="185" alt="img" src="https://github.com/achen4290/ekf-slam/assets/37271059/e440ad65-4ab0-4ab2-b799-6c40fb4e2495">

EKF-SLAM simulation examples. Displays estimated robot and landmark states after 1st set of landmark observations vs. robot and landmark states after 15th set of landmark observations. 

## References 

Our implementation for EKF-SLAM was heavily influenced by this implementation ([EKF-SLAM](https://github.com/Attila94/EKF-SLAM/tree/master)).  


## Summary 
Python package for EKF-SLAM (simultaneous localization and mapping with the extended Kalman filter). Uses known correspondence where once a landmark is observed, the agent knows which landmark the observation belongs to. 



## Installation
`pip install ekf-slam`

## Demo

Demo of one EKF-SLAM run on noisy data (one movement vector and one list of noisy landmark observations). 

```python
import numpy as np 
from ekf_slam import KnownCorrespondence
init = np.array([0, 0, np.pi/2]) # initial robot position 
num_landmarks = 40 # number of landmarks 
Qt = np.array([[0.1, 0], [0, 0.05]]) # sensor noise model 
Rt = 0.05 * np.array([[1, 1, 0], [1, 1, 0], [0, 0, 0]]) # movement noise model 
u_t = np.array([0.5, np.pi/4]) # movement vector 
z = np.array([[2, np.pi/8, 0], [1, np.pi/2, 1], ...]) # list of noisy landmark observations
known_correspondence = KnownCorrespondence(init, num_landmarks, Qt, Rt)
known_correspondence.predict(u_t)
known_correspondence.update(z)
```

## Development
1. Clone this repo
2. Create a new python environment
3. `pip install flit`
4. `flit install -s`

Follow this [README for developers](dev/README.md) for more information about development and simulation. 

[Explanation](https://andrewjkramer.net/intro-to-the-ekf-step-3/) for EKF-SLAM with known correspondence. 


## Documentation
All functions to use EKF-SLAM are contained within the [`KnownCorrespondence`](src\ekf_slam.py) class.

```python
class KnownCorrespondence:
    """
    Class that calculates the EKF-SLAM with known correspondence
    """
    def __init__(self, init: np.ndarray, num_landmarks: int, Qt: np.ndarray, Rt: np.ndarray) -> None:
        """Initializes calculator for 2D EKF-SLAM with known correspondence

        :param init: Array for starting robot position with shape (3,) [x, y, absolute heading in radians]
        :param num_landmarks: Number of total landmarks
        :param Qt: Measurement noise model (2x2)
        :param Rt: Movement nose model (3x3)
        """

    def predict(self, u_t: np.ndarray) -> None:
        """Prediction step of robot state after recieving movement vector; updates robot position and heading only

        u_t[0] = magnitude of movement ()
        u_t[1] = angle of movement in radians

        :param u_t: Movement vector for the robot (2x1)
        """
    def update(self, z: np.ndarray) -> None:
        """Update robot and landmark locations based on observations of landmarks

        :param z: 2D np array of length K, where K is the number of landmarks observed.
            Each row is a 3D vector [distance, angle, landmark ID]
        """
```




