# Real-Time Model Predictive Control for Pendubot Swing-Up

**Pendubot swing-up with SSQP** · euROBIN / CloudPendulum workshop · 24 November 2026

Build a controller that swings up an underactuated two-link robot, catches it upright,
and replans from fresh measurements. This guided, 30-minute notebook takes you from
the robot model to a baseline experiment, a tuning comparison and disturbance recovery.

**[Open the notebook](pendubot_mpc.ipynb)** ·
**[Download the workshop](https://github.com/upatras-lar/2026_cloudpendulum_workshop/archive/refs/heads/main.zip)** ·
**[SSQP documentation](https://lar.upatras.gr/ssqp/)**

## Get started on CloudPendulum

1. Clone this repository in your CloudPendulum workspace, or download and extract the ZIP:

   ```bash
   git clone https://github.com/upatras-lar/2026_cloudpendulum_workshop.git
   cd 2026_cloudpendulum_workshop
   ```

2. Open **`pendubot_mpc.ipynb`** with this folder as the working directory.
   Keep **`pendubot.urdf`** beside it. All six teaching figures are embedded in the
   notebook; no additional assets or support scripts are needed.

3. Select the workshop's CloudPendulum Python kernel. It must provide
   `pycloudpendulumbenchmark`; the disturbance experiment also uses `pycloudpendulum`.
   Install the other dependencies in that same kernel if needed:

   ```python
   %pip install -r requirements.txt
   ```

   The `ssqp` package provides the `ssqpy` import. The notebook also includes an SSQP
   installation cell. Restart the kernel if installation requests it, then run from the top.

4. Work through the notebook in order. Before running the robot, create a local
   **`token.txt`** containing your CloudPendulum token and set your assigned **`CELL_ID`**
   and **`TOKEN_PATH`** in the experiment settings.

5. Set **`RUN_HARDWARE = True`** in the baseline experiment cell when ready.
   The optional recovery experiment has its own **`RUN_DISTURBANCE_ROLLOUT`** switch.
   Both switches start disabled. Keep your token private; `token.txt` is ignored by Git.

NumPy and Matplotlib are enough for the initial geometry and cost activities.
Controller construction and robot experiments require the CloudPendulum interfaces above.

## The 30-minute route

| Time | Activity | What to look for |
|---|---|---|
| 0–5 min | Read the robot, dynamics and costs | Relative joint angles, the passive elbow and the upright goal |
| 5–13 min | Build the model, costs and MPC | How measurements become a shoulder torque |
| 13–21 min | Run a baseline | Motion, score, solver fallbacks and measured timings |
| 21–28 min | Tune one parameter | Predict a change, run it and compare with the baseline |
| 28–30 min | Explore disturbance recovery | A matched reference and shoulder-torque pulse experiment |

Each activity follows **predict → run → explain**, with expandable discussion hints.
The disturbance section can also be used as an extension after the core session.

## What the controller does

The pendubot has two joints and one motor. SSQP plans joint accelerations using
inverse dynamics, constraining the required elbow torque to zero and the shoulder
torque to **±0.1 Nm**. At each update, the controller converts the first planned
acceleration to torque and returns one shoulder command.

The default prediction uses **20 stages at 10 ms** for a **0.20 s horizon**.
The platform is asked for a new command every **1 ms**; use the notebook's measured
timing distributions to assess the actual feedback loop.

For a tuning trial, change only the controller constructor to
`PendubotMPC(velocity_weight=0.06)` or `PendubotMPC(horizon=30)`, then rerun the
experiment and comparison cells. Each trial starts with a fresh controller.
The notebook saves comparison data as `.npz` files beside the notebook.

## Files

| File | Purpose |
|---|---|
| [`pendubot_mpc.ipynb`](pendubot_mpc.ipynb) | Complete workshop, figures, controller and experiments |
| [`pendubot.urdf`](pendubot.urdf) | Two-link robot geometry, inertia and actuator limits |
| [`requirements.txt`](requirements.txt) | Python dependencies to install in the workshop kernel |

## People and links

**Konstantinos Chatzilygeroudis** · Laboratory of Automation and Robotics, University of Patras  
Contact: [costashatz@upatras.gr](mailto:costashatz@upatras.gr)

SSQP is joint work with **[Nick Karydakis](https://github.com/notTypecast)**.

[SSQP documentation](https://lar.upatras.gr/ssqp/) ·
[Laboratory of Automation and Robotics](https://lar.upatras.gr/) ·
[AI Olympics controller repository](https://github.com/upatras-lar/2026_ai_olympics_underactuated_dp)
