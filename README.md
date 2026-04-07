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

#自动加载上一次训练保存的最新模型，从断点继续训练
python scripts/rsl_rl/train.py --task Isaac-Extreme-Parkour-Teacher-Unitree-Go2-v0 --seed 1 --num_envs=1024 --max_iterations 10000 --headless --resume

play教师模型
python scripts/rsl_rl/play.py --task Isaac-Extreme-Parkour-Teacher-Unitree-Go2-Play-v0 --num_envs 16

# 打开旧版vscode，才能连接3090服务器
~/vscode-1.85/code

# sea_nav 训练阶段，容器名称sea_nav
export WANDB_MODE=disabled        # 关闭wandb
# nohup.......> train_log.txt 2>&1 &，在后台运行，关闭vscode也可云训练，xvfb-run -a 在云服务器启动，假装有显示器
nohup xvfb-run -a python -u training/legged_gym/legged_gym/scripts/train.py --num_envs=4096 --headless > train_log.txt 2>&1 &

# sea_nav 的play阶段  此时已经在play.py设置了args.headless = True  RECORD_VIDEO = True  自动录制视频
xvfb-run -a python training/legged_gym/legged_gym/scripts/play.py

# 把录制的视频传到本地的当前文件夹
(base) kil@WP:~/下载$ scp -P 30182 root@sc01-ssh.gpuhome.cc:/root/SEA-Nav-Code/training/legged_gym/logs/Go2_pos_rough/exported/test_20260406_075516.mp4 .
scp -P 30182 root@sc01-ssh.gpuhome.cc:/root/SEA-2.5d/training/legged_gym/logs/Go2_pos_rough/exported/test_20260407_054727.mp4 .

