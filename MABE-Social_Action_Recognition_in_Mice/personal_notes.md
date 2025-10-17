# Notes on MABe Challenge

### The Input (Features)
I get tracking data - basically x,y pixel coordinates of mouse body parts for each frame. Like:

```
Frame 4, Mouse 2:
- body_center: (338.7, 468.4)
- ear_left: (396.8, 460.2)
- ear_right: (329.6, 514.3)
- nose, tail, etc.
```

Different labs track different body parts (some have 18 parts, others like 10).

### The Output (Target)
Need to predict behavioral events - when they happen and who's involved:

```
agent_id: 2 (who's doing the action)
target_id: 2 (who's the target)
action: "rear" (what behavior)
start_frame: 4
stop_frame: 139
```

So basically I'm watching coordinates over time and saying "ok from frame 4 to 139, mouse 2 is rearing"

### Types of Behaviors

**Self-directed**: Mouse doing something to itself
- Ex: Mouse 2 rearing (standing on hind legs) - agent_id=2, target_id=2
- Duration like 4.5 seconds in the example I looked at

**Social**: Mouse interacting with another mouse
- Ex: Mouse 4 avoiding Mouse 2 - agent_id=4, target_id=2
- They were 77.9 pixels apart when it started

Other behaviors: attack, chase, approach, submit, chaseattack, etc.

## The Actual Challenge

- 400+ hours of footage to learn from
- 30+ different behaviors to recognize
- Videos are 25-30 fps (not consistent)
- 2-4 mice per video
- Different labs = different equipment/body parts/quality

### What makes this hard

1. **Temporal detection** - not just "what" but "when does it start/stop"
2. **Generalization** - test mice are totally different from training mice (can't overfit on individual mouse patterns)
3. **Lab variability** - different cameras, arenas, tracking methods
4. **Sparse labels** - not every behavior is labeled in every video
5. **Multiple agents** - tracking 2-4 mice at once

## Key Insights from Leaderboard

Top solutions use:
- **Gradient boosting** (XGBoost/LightGBM) >> deep learning 
- **Heavy feature engineering** - distances, velocities, angles, relative positions
- **Optuna** for hyperparameter tuning
- Some people using unannotated data for unsupervised pretraining (8000 unlabeled videos available)


## Pipeline

1. Load tracking data (x,y coords per frame per mouse)
2. Engineer features (distances between mice, velocities, angles, etc.)
3. Detect behavior segments (not classify every frame)
4. Predict start and stop frames 
5. Format as: video_id, agent_id, target_id, action, start_frame, stop_frame


---

## File Structure Downloads

- train.csv -> list of all videos and basic infos
- train_tracking/{lab}/{video_id}.parquet -> where every mouse body part was at every frame (feautures)
- train_annotation/{lab}/{video_id}.parquet ->  behaviors that happened in this video, time segements, not frame by frame