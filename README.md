# isaacsim-skill
conda activate lab   #激活isaaclab环境

/home/kil/IsaacLab-2.2.1/source/isaaclab_tasks/isaaclab_tasks/manager_based   #放的是可训练的文件
/home/kil/IsaacLab-2.2.1/source/isaaclab_tasks/isaaclab_tasks/manager_based/locomotion/velocity/config/go2/__init__.py   #例如：go2的运动控制强化学习文件  里面会有接口 例如：Isaac-Velocity-Rough-Unitree-Go2-v0崎岖地形的训练

/home/kil/IsaacLab-2.2.1/scripts/reinforcement_learning/rsl_rl  #进行训练的文件夹
/home/kil/IsaacLab-2.2.1/scripts/reinforcement_learning/rsl_rl/train.py   #训练的代码
/home/kil/IsaacLab-2.2.1/scripts/reinforcement_learning/rsl_rl/play.py    #展示训练结果的代码
/home/kil/IsaacLab-2.2.1/scripts/reinforcement_learning/rsl_rl/logs       #存放训练结果的代码

#训练anymal机器人的强化学习导航，task为训练的接口，num_envs=1000为同时训练1000个智能体，headless为无头模式，训练时开启
python train.py --task=Isaac-Navigation-Flat-Anymal-C-v0 --num_envs=1000 --headless  
#展示训练结果，task接口与训练一致，checkpoints为选择训练结果文件，不写为自动展示最新结果
python play.py --task=Isaac-Navigation-Flat-Anymal-C-v0 --num_envs=5 --checkpoint=logs/rsl_rl/anymal_c_navigation/2026-03-21_13-25-51/model_1499.pt  



# robot_lab  使用
python scripts/reinforcement_learning/rsl_rl/train.py --task=RobotLab-Isaac-Velocity-Rough-Unitree-Go2-v0 --headless   #训练
tensorboard --logdir=logs    # 查看奖励等训练曲线

# 想让 Go2 学点新动作，或者走得更稳，整个流程就是这样的：
#改代码： 打开 /home/kil/robot_lab/source/robot_lab/robot_lab/tasks/manager_based/locomotion/velocity/config/quadruped/unitree_go2/rough_env_cfg.py
#修改里面的奖励函数权重、地形参数或者摩擦力。

# 自动找到时间最新的那个文件夹，然后加载里面迭代次数最大的那个 .pt 文件
python scripts/reinforcement_learning/rsl_rl/play.py --task=RobotLab-Isaac-Velocity-Rough-Unitree-Go2-v0  
# 读取历史特定模型
python scripts/reinforcement_learning/rsl_rl/play.py --task=RobotLab-Isaac-Velocity-Rough-Unitree-Go2-v0 --load_run 2026-03-24_14-30-00 --checkpoint model_100.pt



# Isaaclab_Parkour 使用
链接 https://github.com/CAI23sbP/Isaaclab_Parkour
--num_envs=1024 # 原本为4096,为了缩短时间我改了   --max_iterations 10000   #原本为50000,我改了
