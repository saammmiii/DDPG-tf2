# Deep Deterministic Policy Gradient (DDPG) in Tensorflow 2

![python 3](https://img.shields.io/badge/python-3-blue.svg)
![tensorflow 2](https://img.shields.io/badge/tensorflow-2-orange.svg)

Looking at Reinforcement Learning, there are two kinds of action space, namely discrete and continuous. The continuous action space represents the continuous movement a robot can have when actuating. I was bias towards the continuous one at the time of having the idea to write the DDPG implementation. I find that the continuous one can provide a smoother movement, which may be benefitial to control robotic actuator. DDPG is an approach to do so. The source code is available here https://github.com/samuelmat19/DDPG-tf2

My implementation of DDPG based on paper https://arxiv.org/abs/1509.02971, but also highly inspired by https://spinningup.openai.com/en/latest/algorithms/ddpg.html . This implementation is simple and can be used 
as a boilerplate for your need. It also modifies a bit the original algorithm which mainly aims to speed up the training
process. I would highly recommend to use Spinning Up library as it provides more algorithm options. This repository is suitable if direct modification to Tensorflow 2 model or simple training API is favorable.

Several videos of proof-of-concepts are as such:
- [AI learns how to invert pendulum under 8 minutes](https://youtu.be/lY99ye4hhok)
- [AI controls the Lunar Lander on Open AI Gym](https://youtu.be/-FMuvFVskBM)
- [AI speed walks on Open AI Gym's BipedalWalker-v3](https://youtu.be/B95WjH4EP9I)

##### Table of Contents  
- [Why?](#why)  
- [Changes from original paper](#changes-from-original-paper)
- [Requirements](#requirements)
- [Training](#training)
- [Sampling](#sampling)
- [Future improvements](#future-improvements)
- [CONTRIBUTING](#contributing)
- [LICENSE](#license)

## Why?
Reinforcement learning is important when it comes to real environment. As
there is no definite right way to achieve a goal, the AI can be optimized based
on reward function instead of continuously supervised by human.

In continuous action space, DDPG algorithm shines as one of the best in
the field. In contrast to discrete action space, 
continuous action space mimics the reality of the world.

The original implementation is in PyTorch. Additionally, there are several
modifications of the original algorithm that may improve it.

## Changes from original paper
As mentioned above, there are several changes with different aims:
- The loss function of Q-function uses **Mean Absolute Error** instead of Mean
Squared Error. After experimenting, this speeds up training by 
a lot of margin. One possible cause is because Mean Squared Error may
overestimate value above one and underestime value below one (x^2 function).
This might be unfavorable for the Q-function update as all value range should
be treated similarly.
- **Epsilon-greedy** is implemented in addition to the policy's action. This
increases faster exploration. Sometimes the agent can stuck with one policy's
action, this can be exited with random policy action introduced by epsilon-greedy.
As DDPG is off-policy, this surely is fine. The epsilon-greedy and noise are turned off in the testing state.
- **Unbalance replay buffer**. Recent entries in the replay buffer are more likely to be taken
than the earlier ones. This reduces repetitive current mistakes that the agent
does.

## Requirements
`pip3 install ddpg-tf2`

## Training

```python3
ddpg-tf2 --train True --use-noise True
```

After every epoch, the network's weights will be stored in the checkpoints directory defined in `common_definitions.py`.
There are 4 weights files that represent each networks, namely critic network,
actor network, target critic, and target actor. 
Additionally, TensorBoard is used to track the resultive losses and rewards.

The pretrained weights can be retrieved from these links:
- [BipedalWalker-v3](https://github.com/samuelmat19/DDPG-tf2/releases/download/0.0.1/Bipedal_checkpoints.zip)
- [LunarLanderContinuous-v2](https://github.com/samuelmat19/DDPG-tf2/releases/download/0.0.2/Lunar_checkpoints.zip)

## Testing (Sampling)

Testing is done by the similar executable, but with
specific parameters as such. If the weight is available in the checkpoint folder, it will load the weight automatically from there.

```python3
ddpg-tf2 --train False --use-noise False
```

## Future improvements
- [ ] Improve documentation
- [x] GitHub Workflow
- [x] Publish to PyPI

## CONTRIBUTING
To contribute to the project, these steps can be followed. Anyone that contributes will surely be recognized and mentioned here!

Contributions to the project are made using the "Fork & Pull" model. The typical steps would be:

1. create an account on [github](https://github.com)
2. fork this repository
3. make a local clone
4. make changes on the local copy
5. commit changes `git commit -m "my message"`
6. `push` to your GitHub account: `git push origin`
7. create a Pull Request (PR) from your GitHub fork
(go to your fork's webpage and click on "Pull Request."
You can then add a message to describe your proposal.)


## LICENSE
This open-source project is licensed under MIT License.
