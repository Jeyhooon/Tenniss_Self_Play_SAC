[//]: # (Image References)
[image1]: images/trained_sac_self_play_tennis_agent.gif "Trained Agent"
[image2]: images/learning_curve.png "Learning Curve"

# Tennis Self-paly with SAC (Continuous)

![Trained Agent][image1]

### Project Details
This project is about training a policy-gradient-based agent that plays against itself in a tennis game shown in the figure above. \
In this environment, two agents control rackets to bounce a ball over a net. If an agent hits the ball over the net, it receives a reward of +0.1. 
If an agent lets a ball hit the ground or hits the ball out of bounds, it receives a reward of -0.01. Thus, the goal of each agent is to keep the ball in play.

The observation space consists of 8 variables corresponding to the position and velocity of the ball and racket 
and each agent receive a stack of past three time steps (hence observation vector is of size 24). Two continuous actions are available, corresponding to movement toward (or away from) the net, and jumping.

Each agent receives its own, local observation; therefore, it's possible to share the policy and value networks and the experiences are stored in a common replay buffer. 


For this project, the Unity environment with two agents is used and trained using data efficient off-policy policy gradient algorithm; [soft-actor-critic (SAC)](https://arxiv.org/abs/1801.01290). 
SAC tries to maximize expected reward while also maximizing entropy. That is, to succeed at the task while acting as randomly as possible.

The task is episodic, and in order to solve the environment, the agents must get an average score of +0.5 (over 100 consecutive episodes, after taking the maximum over both agents). Specifically:

* After each episode, we add up the rewards that each agent received (without discounting), to get a score for each agent. This yields 2 (potentially different) scores. 
* We then take the maximum of these 2 scores.
This yields a single score for each episode.

The environment is considered solved, when the average (over 100 episodes) of those scores is at least +0.5.
## Getting Started

### How to Install:
* Clone this repository: `git clone https://github.com/Jeyhooon/Tenniss_Self_Play_SAC.git`.  
* `cd` to the repository folder (e.g.: `cd ~/git/Tenniss_Self_Play_SAC`)
* Create a virtual environment using [virtual-env](https://docs.python.org/3/tutorial/venv.html#creating-virtual-environments) (e.g.: `virtualenv venv -p python3.6`)
* Source the virtual environment you just create: e.g.: `source venv/bin/activate`
* Install the requirements using pip command: `pip install -r requirements.txt`

### Downloading the Unity Environment
#### (In case your system is other than linux so the one included in the repo won't work!)
Download the environment from one of the links below.  You need only select the environment that matches your operating system:\
(copy the unzipped folder to this directory: `self_play_sac/data/` and update the `ENV_NAME` field in the config file inside the `train.py` appropriately)\
    - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip) \
    - Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis.app.zip) \
    - Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86.zip) \
    - Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86_64.zip) \
    
(_For Windows users_) Check out [this link](https://support.microsoft.com/en-us/help/827218/how-to-determine-whether-a-computer-is-running-a-32-bit-version-or-64) if you need help with determining if your computer is running a 32-bit version or 64-bit version of the Windows operating system.

(_For AWS_) If you'd like to train the agent on AWS (and have not [enabled a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md)), 
then please use [this link](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux_NoVis.zip) to obtain the "headless" version of the environment.  
You will **not** be able to watch the agent without enabling a virtual screen, but you will be able to train the agent.  \
(_To watch the agent, you should follow the instructions to [enable a virtual screen](https://github.com/Unity-Technologies/ml-agents/blob/master/docs/Training-on-Amazon-Web-Service.md), and then download the environment for the **Linux** operating system above._)

### Instructions
Run `python trainer.py` in the terminal (with active `venv`) to train the agent; hyperparameters can be changed inside the `trainer.py` script (code is adapted from: [mimoralea/gdrl](https://github.com/mimoralea/gdrl)):
```Python
config = {
    "ROOT_DIR": "results",                  # directory to save the results
    "BUFFER_SIZE": int(1e6),                # replay buffer size
    "BATCH_SIZE": 256,                      # mini-batch size
    "WARMUP_BATCHES": 10,                   # number of initial batches to fill the buffer with
    "TAU": 1e-3,                            # for soft update of target parameters
    "UPDATE_EVERY": 1,                      # how often to update the network
    "SEED": [1],                            # list of the seed to do randomize each training
    "Q_NET_Hidden_Dims": (128, 128),        # Size of the hidden layer in Q-Net
    "Q_LR": 7e-4,                           # Q-Net learning rate
    "Q_MAX_GRAD_NORM": float(100.0),        # to clip gradients of Q-Net
    "POLICY_NET_Hidden_Dims": (64, 64),     # Size of the hidden layer in Policy-Net
    "POLICY_LR": 5e-4,                      # Policy-Net learning rate
    "POLICY_MAX_GRAD_NORM": float(100.0),   # to clip gradients of the Policy-Net

    "ENV_SETTINGS": {
            'ENV_NAME': '../data/Tennis_Linux/Tennis.x86_64',
            'GAMMA': 0.99,
            'MAX_MINUTES': 60,
            'MAX_EPISODES': 2000,
            'GOAL_MEAN_100_REWARD': 0.5
        }
}
```
In case not want to train the agent but to watch the smart agent, run `trainer.py` with `--is_training False` argument and provide the load_dir argument to load the trained agent; e.g.: `--load_dir results/2021_10_21/2021-10-21_20_20_36/model_final.pth`.

### Implementation Details
Please refer to `Report.ipynb` for learning more about the implementation details.

### Results
You would get a learning curve similar to the one below and the trained agent would look like the one shown in the beginning of this file (see the gif image above):
![Learning Curve][image2]
