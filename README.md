# navbot
    It's a collection for mapless robot navigation using RGB image as visual input. It contains the test 
    environment and motion planners, aiming at realizing all the three levels of mapless navigation:
    1. memorizing efficiently; 
    2. from memorizing to reasoning; 
    3. more powerful reasoning
    The experiment data is in ./materials/record folder. 
    This work is under review of IEEE ITSC 2019.
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/mapless%20motion%20planner.png"  align='center' width="600"> <br>

## Environment 
<font face="Times New Roman">I built the environment for testing the algorithms.</font> <br>

<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/environment.PNG"  align='center' width="600">    
<font face="Times New Roman">It has the following properties:</font> 

*  Diverse complexity. 
*  Gym-style Interface.
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/Interface.PNG" align='center' width="500"> 

*  Support ROS.

## Memorizing
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/E2E_PPO_nav2.gif" align='center' width="500"> 

### VAE
#### Structure 

<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/vae.png" align='center' width="200"> 

#### Result 

<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/vae_show.png" align='center' width="500"> 

### VAE-based Proposed Planner Compared with  benchmark 
1. The proposed is blue trajectory and the benchmark is green. <br>
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/memorize_trajectory.PNG" align='center' width="300"> <br>
2. The success rate comparision in maze1. <br>
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/result/maze1_dense_success.png" width="500"> 

## From Memorizing to Reasoning
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/PPO_rnn_nav2.gif" align='center' width="500"> 

### Stacked LSTM and network structure
Stacked LSTM <br>
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/stacked%20LSTM.png" width="500"> <br>
network structure <br>
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/images/rnn.png" width="400"> 
### Result
Success rate in maze1 <br>
<img alt="" src="https://github.com/marooncn/navbot/blob/master/materials/result/maze1_rnn_success.png" width="500"> 




## Install
#### Ddependencies
tensorflow: 1.5.0 <br>
OS: Ubuntu 16.04  <br>
Python: 2.7 <br>
ROS: Kinetic  <br>
Gazebo: 7   <br>
tensorforce: https://github.com/tensorforce/tensorforce  <br>
    
#### Run
    sudo apt-get install ros-kinetic-gazebo-ros-pkgs ros-kinetic-gazebo-ros-control
    sudo apt-get install ros-kinetic-turtlebot-*
    sudo apt-get remove ros-kinetic-turtlebot-description
    sudo apt-get install ros-kinetic-kobuki-description
    # change to catkin_ws/src
    git clone https://github.com/marooncn/navbot
    cd ..
    catkin_make
    source ./devel/setup.bash
    # you can change the configure in config.py
    cd src/navbot/rl_nav/scripts
    # run the proposed model for memorizing
    python PPO.py
    # run the proposed model for reasoning
    python E2E_PPO_rnn.py
#### Details
1. The default environment is maze1, you need to change maze_id in [nav_gazebo.launch](https://github.com/marooncn/navbot/blob/master/rl_nav/launch/nav_gazebo.launch) and [config.py](https://github.com/marooncn/navbot/blob/master/rl_nav/scripts/config.py) if you want change the environment. <br>
2. maze1 and maze2 are speeded up 10 times to train, if you want speed up other environments, just change 

       <max_step_size>0.001</max_step_size>
       <real_time_factor>1</real_time_factor>
   to
 
       <max_step_size>0.01</max_step_size>
       <!-- <real_time_factor>1</real_time_factor> -->
   in the environment file in [worlds](https://github.com/marooncn/navbot/tree/master/rl_nav/worlds).
3. To reproduce the result, please change the related parameters in [config.py](https://github.com/marooncn/navbot/blob/master/rl_nav/scripts/config.py) according to [config.txt](https://github.com/marooncn/navbot/blob/master/materials/record/config.txt).
4. PPO is not a deterministic policy gradient algorithm, the action at every timestep is sampled according to the distribution. It can be seen as "noise" and it's useful for explorations and generalizations. If you want to use the best strategy after the model is trained, just change 'deterministic = True' in [config.py](https://github.com/marooncn/navbot/blob/master/rl_nav/scripts/config.py) and the performance will be improved.

## Reference
[tensorforce](https://github.com/tensorforce/tensorforce)([blog](https://www.jiqizhixin.com/articles/2017-07-14-7?from=synced&keyword=tensorforce)) <br>
[gym_gazebo](https://github.com/erlerobot/gym-gazebo/blob/master/gym_gazebo/envs/turtlebot/gazebo_circuit2c_turtlebot_camera_nn.py) <br>
[gazebo](http://wiki.ros.org/gazebo) <br>
[roslaunch python API](http://wiki.ros.org/roslaunch/API%20Usage) <br>
[turtlebot_description](https://github.com/turtlebot/turtlebot/tree/kinetic/turtlebot_description) <br>
[kobuki_description](https://github.com/yujinrobot/kobuki/tree/devel/kobuki_description) <br>
[WorldModelsExperiments(official)](https://github.com/hardmaru/WorldModelsExperiments)  <br>
[WorldModels(by Applied Data Science)](https://github.com/AppliedDataSciencePartners/WorldModels) <br>
