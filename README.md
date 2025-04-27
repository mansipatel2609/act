# Touch‑Two‑Blocks Task – ACT Modifications

This patch adds a new simulated task where the bimanual arm must **touch Block A → Block B → Block A** in sequence.

## Files

* **sim_env_patch.diff** – patch to `sim_env.py`  
  * adds `TouchTwoBlocksSequenceTask`  
  * registers new task name `sim_touch_two_blocks` in `make_sim_env`

* **assets/bimanual_viperx_touch_two_blocks.xml** – MuJoCo model with two free blocks

## How to apply

```bash
# From the root of your cloned ACT repo
git apply sim_env_patch.diff

# Copy the XML into the assets folder
cp assets/bimanual_viperx_touch_two_blocks.xml assets/
```

When collecting scripted or tele‑operated data:

```python
from sim_env import make_sim_env, BOX_POSE

# Provide 14 initial qpos values (two 7‑DoF free joints)
BOX_POSE[0] = [0.20, 0.50, 0.05, 1, 0, 0, 0,
               0.26, 0.50, 0.05, 1, 0, 0, 0]

env = make_sim_env('sim_touch_two_blocks')
ts = env.reset()
...
```

Reward structure (success = 3):
1. Touch **Block A** → reward = 1  
2. Touch **Block B** → reward = 2  
3. Touch **Block A** again → reward = 3  

Enjoy!