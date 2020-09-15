
### Project Continuous Control - Report

#### 1. Overview Of The Project

> In this project our goal is to train a agent i.e double jointed arm to move to target location. A reward of 0.1 is provided for every time step the arm is in the target location thus enabling the agent to maintain its position at the target location.
We have 33 possible states corresponding to position, rotation, velocity, and angular velocities of the arm and action space is a vector with four numbers, corresponding to torque applicable to two joints. Every entry in the action vector should be a number between -1 and 1.

#### 2. Algorithm Explanation

##### Deep Deterministic Policy Gradients :
> DDPG uses four neural networks: a Q network, a deterministic policy network, a target Q network, and a target policy network.

![image.png](attachment:1.png)

> The Q network and policy network is very much like simple Advantage Actor-Critic, but in DDPG, the Actor directly maps states to actions (the output of the network directly the output) instead of outputting the probability distribution across a discrete action space

> The target networks are time-delayed copies of their original networks that slowly track the learned networks. Using these target value networks greatly improve stability in learning. Here’s why: In methods that do not use target networks, the update equations of the network are interdependent on the values calculated by the network itself, which makes it prone to divergence.

![2.png](attachment:image.png)

##### DDPG Algorithm:

![3.png](attachment:image.png)

##### Replay Buffer:
> As used in Deep Q learning (and many other RL algorithms), DDPG also uses a replay buffer to sample experience to update neural network parameters. During each trajectory roll-out, we save all the experience tuples (state, action, reward, next_state) and store them in a finite-sized cache — a “replay buffer.” Then, we sample random mini-batches of experience from the replay buffer when we update the value and policy networks.

##### Actor (Policy) & Critic (Value) Network:
> The value network is updated similarly as is done in Q-learning. The updated Q value is obtained by the Bellman equation:

![4.png](attachment:image.png)

> However, in DDPG, the next-state Q values are calculated with the target value network and target policy network. Then, we minimize the mean-squared loss between the updated Q value and the original Q value:
![5.png](attachment:image.png)

> For the policy function, our objective is to maximize the expected return:
![6.png](attachment:image.png)

> To calculate the policy loss, we take the derivative of the objective function with respect to the policy parameter. Keep in mind that the actor (policy) function is differentiable, so we have to apply the chain rule.
![7.png](attachment:image.png)

> But since we are updating the policy in an off-policy way with batches of experience, we take the mean of the sum of gradients calculated from the mini-batch:
![8.png](attachment:image.png)

#### Hyperparameters Used :
<p>
OU_SIGMA = 0.2
    
OU_THETA = 0.15

LEARN_EVERY = 20 

LEARN_NUM = 10  

GRAD_CLIPPING = 1.0  

BUFFER_SIZE = int(1e6) 

BATCH_SIZE = 128   

WEIGHT_DECAY = 0     

EPSILON = 1.0        

EPSILON_DECAY = 1e-6

GAMMA = 0.99        

TAU = 1e-3          

LR_ACTOR = 1e-3      

LR_CRITIC = 1e-3  

</p>

#### Network Architecture:
> Here we have two different networks for actor and critic. Below i have mentioned the architectures of the both networks.

                Actor:
>                   fc1(400 units) -- batch_normalization -- fc2(300 units) -- fc3(4 units)

              Critic:
>                   fc1(400 units) -- batch_normalization -- fc2(300 units + 4 units) -- fc3(1 units)

> Here we have used relu as the activation function and adam as the optimizer.

#### Result:
> It took 564 episodes to solve the environment with an Average score: 30.02
![9.png](attachment:image.png)

#### Ideas For Further Improvement:
> Algorithms such as PPO,A3C can also be considered for these kind of problems.
> Experiment with different values for hyperparameters such as fc1 units, fc2 units, batch_size etc.
> Adding a few additional layers to the actor and critic networks.


```python

```
