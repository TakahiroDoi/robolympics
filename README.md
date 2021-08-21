# Readme
* 2021 Neuromatch Academy Deep Learning Course's project 
* Training virtual one-leg hopper (from the PyBullet environment, an open source implementation of OpenAI Gym) using deep reinforcement learning 
* We used D4PG RL agent (https://arxiv.org/pdf/1804.08617.pdf) from dm-acme
* Project team (Robojockeys)
  * Main contributors: Takahiro Doi, Yuchen Zhao, Shuqi Liu, Amir Kashef
  * Honorary member: Quinn Lee (, who chose the gym environment for the course project)
* Presentation slide https://docs.google.com/presentation/d/1BdSZX5UiMUoOZi3DcjCXo22JGeFoAlqX4VmcxjqS1B4/edit#slide=id.ge8ced83644_4_11
* Colab notebook: https://colab.research.google.com/drive/1jdI2hsYZlDBAZB8m_JI7vHrhoHcSmckq?usp=sharing
* Template provided at the beginning of the course: https://deeplearning.neuromatch.io/projects/ReinforcementLearning/robolympics.html

# Summary of the project progress during Neuromatch Academy
1. Moving forward (MF)
  * We struggled to find a good set of hyper parameters to just teach Hopper to hop forward (it stood still but did not move forward.)
    * Quinn said: it didn't learn to hop forward even after trained for 1 million steps 
  * Eventually found that Hopper learned this task in ~200,000 steps with the default RL algorithm, architecture, and hyper parameters (in most cases).
  * This Hopper could hop forward but had two _problems_:
    * when reaching the target destination, it immediately fell and couldn't stand still
    * when given the target destination, it couldn't stay still there 
    
2. Moving forward (MF) and stand still (SS)
  * We set out to solve the two issues above.
  * Strategy: two goals (two target destinations) were mixed during the training (target distance = 0, 1000)
    * 10 episodes / block, 100 blocks   
    * Coefficient for alive bonus was later increased from 1 to 2, too (for better stability)
  * It worked, although Hopper moved only very slowly 
  * Solved the two issues above 
  * Implemented a test condition where the goal was shifted further when reached  
  * Hopper sequenced the learned moves: MF->SS->MF->SS 
  
3. MF, SS, and MB (Moving Backward) 
  * The same architecture seemed not working even with million steps  
  * Tried a deeper model and wider model  (the first layer width doubled for both actor and critic)
  * Alive coefficient (in reward) increased from 2 to 3
  * Unstable but Hopper learned to do three different moves (with 2 million step)!   

# Future direction 
  * Increase stability (for example, the average episode length is quite shorter than max=1000)
  * Still task confusion is seen, so try to fix it 
  * Things we can try: more training, more initial conditions, shorter block size (better continuous learning??)  

# Improve presentations  
  * Learning curves must be included  
    * Return, episode length, average velocity (for moving tasks) 

# Usage 
## Key parameters 
### Training 
* Architecture (network width and depth)
* idx_task_type: target locations   
* idx_task_type: target locations   
* save_agent_name and load_agent_name: for checkpoints  
* idx_training_design: blocked design or not  
* idx_training_design: choose which way you want to train the hopper 

### Testing  
* walk_target: walk destination during test  
* idx_target_dynamic: 1 for shifting the target location further away (use to make Hopper to do MF->SS->MF->SS move)
* b_create_video: 1 for creating a video, 0 for running many tests for analysis  

### Checkpoint  
0. Delete current agent if any 
1. Make agent instance  
2. Reload file  
3. Make agent instance again 
* For a long training, I usually save the agent with a new name, re-load it, then start training to be safe
