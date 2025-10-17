# MABe Challenge - Social Action Recognition in Mice

## Competition Overview

### Description

Animal social behavior is complex. Species from ants to wolves to mice form social groups where they build nests, raise their young, care for their groupmates, and defend their territory. Studying these behaviors teaches us about the brain and the evolution of behavior, but the work has usually required subjective, time-consuming documenting of animals' actions. ML advancements now let us automate this process, supporting large-scale behavioral studies in the wild and in the lab.

But even automated systems suffer from limited training data and poor generalizability. In current methods, an experimenter must hand-label hundreds of new training examples to automate recognition of a new behavior, which makes studying rare behaviors a challenge. And models trained within one research group usually fail when applied to data from other studies, meaning there is no guarantee that two labs are really studying the same behavior.

This competition challenges you to build models to identify over 30 different social and non-social behaviors in pairs and groups of co-housed mice, based on markerless motion capture of their movements in top-down video recordings. The dataset includes over 400 hours of footage from 20+ behavioral recording systems, all carefully labeled frame-by-frame by experts. Your goal is to recognize these behaviors as accurately as a trained human observer while overcoming the inherent variability arising from the use of different data collection equipment and motion capture pipelines.

Your work will help scientists automate behavior analysis and better understand animal social structures. These models may be deployed across numerous labs, in neuroscience, computational biology, ethology, and ecology, to create a foundation for future ML and behavior research.

### Evaluation

This competition uses an F-Score variant as the metric. The F scores are averaged across each lab, each video, and score only the specific behaviors and mice that were annotated for a specific video. You may wish to review the full implementation in the MABe F Beta metric notebook.

#### Submission File Format

You must create a row in the submission file for each discrete action. The file should contain a header and have the following format:

```csv
row_id,video_id,agent_id,target_id,action,start_frame,stop_frame
0,101686631,mouse1,mouse2,sniff,0,10
1,101686631,mouse2,mouse1,sniff,15,16
2,101686631,mouse1,mouse2,sniff,30,40
3,101686631,mouse2,mouse1,sniff,55,65
```

### Timeline

- **September 18, 2025** - Start Date
- **December 8, 2025** - Entry Deadline (must accept competition rules before this date)
- **December 8, 2025** - Team Merger Deadline (last day to join or merge teams)
- **December 15, 2025** - Final Submission Deadline

All deadlines are at 11:59 PM UTC on the corresponding day unless otherwise noted.

### Prizes

- First Prize: $20,000
- Second Prize: $10,000
- Third Prize: $8,000
- Fourth Prize: $7,000
- Fifth Prize: $5,000

**Total Prize Pool: $50,000**

### Code Requirements

Submissions to this competition must be made through Notebooks. Requirements:

- CPU Notebook <= 9 hours run-time
- GPU Notebook <= 9 hours run-time
- Internet access disabled
- Freely & publicly available external data is allowed, including pre-trained models
- Submission file must be named `submission.csv`

### Previous Efforts and Benchmarks

This competition builds on the Multi-Agent Behavior (MABe) Workshop and Competitions in 2021 and 2022, which focus on supervised and representation learning using data from multiple labs and organisms.

**Related Papers:**
- CalMS21 at NeurIPS: https://arxiv.org/pdf/2104.02710.pdf
- MABe22 at ICML 2023: https://arxiv.org/pdf/2207.10553.pdf
- CRIM13 at CVPR: doi: 10.1109/CVPR.2012.6247817

**Note:** The competition provides all pose and annotation files from CalMS21, MABe22, and CRIM13 as additional data in the training set.

---

## Data

### Dataset Description

Annotating the behavior of mice is currently a tremendously time consuming behavior. This competition challenges you to automate this process and unlock new insights into animal behavior.

This competition uses a hidden test set. When your submitted notebook is scored, the actual test data (including a full length sample submission) will be made available to your notebook. **Expect to see roughly 200 videos in the hidden test set.**

### Files

#### [train/test].csv
Metadata about the mice and recording setups.

- `lab_id` - The pseudonym of the lab that provided the data. The CalMS21, CRIM13, and MABe22 datasets are copies of publicly available datasets provided as additional training data. Some tracking files in CalMS21 set are largely duplicated-- these were annotated by multiple individuals for different sets of behaviors.
- `video_id` - A unique identifier for the video.
- `mouse[1-4] [strain/color/sex/id/age/condition]` - Basic information about each mouse.
- `frames per second`
- `video duration (sec)`
- `pix per cm (approx)`
- `video [width/height]`
- `arena [width/height] (cm)`
- `arena shape`
- `arena type`
- `body parts tracked` - Each lab used different technology to track their mice, so the specific body parts tracked may vary.
- `behaviors labeled` - The behaviors labeled in this video. Necessary as the annotations are sparse. Each lab annotated their videos differently, may not have annotated all behaviors for a video, and may not have annotated all mice in each video.
- `tracking method` - The model used to track the animal's pose.

#### [train/test]_tracking/
The feature data.

- `video_frame`
- `mouse_id`
- `bodypart` - The tracked body part. Different labs may track different body parts
- `[x/y]` - The body part's X/Y coordinate position in pixels.

#### train_annotation/
Train labels.

- `agent_id` - ID of the mouse performing the behavior.
- `target_id` - ID of the mouse targeted by the behavior. Some behaviors, such as a mouse grooming itself, have the same agent and target ID.
- `action` - What behavior is occurring (e.g., grooming, chasing). Different labs annotated different behaviors.
- `[start/stop]_frame` - The first/last frame of the action.

#### sample_submission.csv
A submission file in the correct format.

- `row_id` - You'll need to provide a column of unique row IDs. A simple enumeration will suffice.
- `video_id`
- `agent_id`
- `target_id`
- `action`
- `[start/stop]_frame`

### Dataset Statistics

- **Files:** 9,640 files
- **Size:** 2.84 GB
- **Type:** parquet, csv
- **License:** Attribution 4.0 International (CC BY 4.0)

---

## Key Rules Specific to This Competition

### Team & Submission Limits
- **Maximum team size:** 5 members
- **Team mergers:** Allowed until Team Merger Deadline (Dec 8, 2025)
- **Submission limits:** Maximum 5 submissions per day
- **Final submissions:** Select up to 2 final submissions for judging

### External Data and Tools
- External data **allowed** if publicly available and equally accessible to all participants at no cost
- Must meet **"Reasonableness Standard"** - costs should not be excessive
- **Example (acceptable):** Small subscription charges (e.g., Gemini Advanced)
- **Example (NOT acceptable):** Proprietary dataset license exceeding prize value
- **Automated Machine Learning Tools (AMLT):** Allowed with appropriate license

### Winner Obligations
- Must provide **detailed methodology description** including:
  - Architecture
  - Preprocessing
  - Loss function
  - Training details
  - Hyperparameters
- Must provide **code repository** with complete instructions for reproducibility
- Winning submission must be licensed under **CC BY 4.0** (open source)
- Must deliver training code, inference code, and computational environment description

### Data License
- Competition data: **CC BY 4.0**
- Can be used for commercial or non-commercial purposes
- Must maintain data security and not share with non-participants

---

## Discussion Highlights

### Important Insights from Community

#### Mouse IDs and Test Set
- `mouse_id` is just a mouse identifier (not a categorical variable)
- **Test set contains completely different animals from training set** (statistically independent test)
- This prevents overfitting on individual animals' idiosyncratic movement patterns
- Implication: Don't rely too heavily on mouse_id as a feature

#### Conditions Field Explanation
- MABe22 datasets tracked animals 24 hours/day for 4 days
- "day 1 - 00:40 (lights off)" means 12:40am on day 1
- **Mice are nocturnal with circadian rhythms**, so timestamp provides info about likely behavioral states
- This temporal information can be useful for modeling

#### Missing Annotations (MABe22 datasets)
- Approximately **8,000 of 8,800 provided "videos" are not labeled**
- Only a small subset of original MABe22 dataset has hand annotations
- **Unannotated pose data can still be valuable** for unsupervised feature learning prior to classification
- **Winning solution from 2021 competition** used unannotated pose data for unsupervised feature learning
- Can refer to original MABe22 paper to create annotations using heuristics

#### External Resources
- External datasets are allowed (see rules tab for details)
- Self-trained models (e.g., LightGBM) hosted publicly on GitHub are allowed, as long as they're trained using only permitted data
- Official Discord available at discord.gg/kaggle for casual discussion
- **Important:** Kaggle Staff and Hosts will NOT monitor Discord - keep important questions on Kaggle forums

---

## Solutions from Other Competitors

### Top Leaderboard Performers

**Current Top 5 Teams:**
1. **Jack Vittimberga** - Score: 0.47 (11 entries)
2. **Bửu Phạm Quốc** - Score: 0.47 (13 entries)
3. **Youri Matiounine** - Score: 0.46 (45 entries)
4. **Evan Arlen Handy** - Score: 0.45 (31 entries)
5. **tenten** - Score: 0.45 (29 entries)

**Note:** Leaderboard is calculated with approximately 32% of test data. Final results will be based on the other 68%, so standings may change significantly.

### Public Notebooks and Approaches

#### 1. Optuna tuning 0.41 (Score: 0.41)
**Author:** Fruit19  
**Approach:**
- Based on Taylor S. Amarel's baseline notebook
- Added **Optuna hyperparameter tuning** for XGBoost and LightGBM
- Comprehensive feature engineering with hyperparameter optimization
- Runtime: ~33 minutes

**Key Techniques:**
```python
# Optuna for automated hyperparameter optimization
USE_OPTUNA = True  # Set to True to run hyperparameter optimization
OPTUNA_TRIALS = 100  # Number of optimization trials
```
- XGBoost and LightGBM ensemble
- Full feature engineering pipeline
- Systematic hyperparameter search

#### 2. MAYBE REMIX: Extra Features (Score: 0.41)
**Medals:** 62 Silver  
**Approach:**
- Extended feature engineering beyond baseline
- Based on remix of existing solutions
- Focus on creating additional derived features from pose data

#### 3. 2.5D CNN for Mouse Social Action Detection (Score: 0.23)
**Medals:** 19 Bronze  
**Approach:**
- Uses **2.5D Convolutional Neural Networks**
- Treats pose tracking data in a spatial-temporal manner
- Lower score suggests deep learning approaches may need more tuning or different architecture
- May require more data augmentation or pre-training

#### 4. MABe-NN-Motion-Features (Score: 0.3)
**Approach:**
- Neural network with **motion-based features**
- Focus on extracting movement patterns from pose data
- Computes velocity, acceleration, and trajectory features

#### 5. MABe SOLUTION #2 (Score: 0.41)
**Author:** Based on Jakup Ymeraj's work  
**Approach:**
- Achieves competitive 0.41 score
- Likely uses gradient boosting methods
- Focus on feature engineering

### Common Successful Approaches

1. **Feature Engineering is King**
   - Most successful solutions focus heavily on feature engineering from pose tracking data
   - Extract relative positions, distances, angles between mice
   - Compute velocity, acceleration, angular velocity
   - Create interaction features (distance between mice, relative orientations)

2. **Gradient Boosting Dominates**
   - **XGBoost** and **LightGBM** appear to be the most popular and successful choices
   - Consistently achieving 0.41+ scores
   - Better performance than deep learning approaches so far

3. **Hyperparameter Tuning**
   - **Optuna** is being used for systematic hyperparameter optimization
   - Significant score improvements from proper tuning
   - Automated search more effective than manual tuning

4. **Ensemble Methods**
   - Combining multiple models (XGBoost + LightGBM)
   - Averaging predictions across different feature sets
   - Stacking different model types

5. **Motion and Temporal Features**
   - Extracting velocity, acceleration from raw pose coordinates
   - Computing relative positions and movements between mice
   - Temporal smoothing and rolling window statistics
   - Frame-to-frame differences

### Notable EDA Notebooks

These notebooks provide valuable insights into data structure and feature ideas:

- **Fast EDA and Fast Baseline** (16 Bronze) - Quick overview and baseline implementation
- **🐭 MABe EDA: Behavior Analysis & Features** (12 Bronze) - Detailed behavior analysis
- **Visualizing Mouse Pose Track🐭** (23 Bronze) - Visualization of pose tracking data
- **MABe — Dataset EDA** - Comprehensive exploratory data analysis

### Key Takeaways for Competitors

1. **Start with gradient boosting** (XGBoost/LightGBM) rather than deep learning
2. **Invest heavily in feature engineering** - this is where most gains come from
3. **Use Optuna** for hyperparameter optimization rather than manual tuning
4. **Leverage unannotated data** for unsupervised pre-training or feature learning
5. **Consider temporal context** - behaviors unfold over time, not just single frames
6. **Handle lab variability** - different labs have different tracking methods and body parts
7. **Pay attention to sparse annotations** - not all behaviors are labeled in all videos
8. **Test set has different mice** - avoid overfitting to individual mouse characteristics

---

## Additional Resources

### Competition Links
- **Competition Homepage:** https://www.kaggle.com/competitions/MABe-mouse-behavior-detection
- **Official Discord:** discord.gg/kaggle
- **Metric Implementation:** MABe F Beta notebook on Kaggle

### Academic Papers
- CalMS21 (NeurIPS): https://arxiv.org/pdf/2104.02710.pdf
- MABe22 (ICML 2023): https://arxiv.org/pdf/2207.10553.pdf
- CRIM13 (CVPR): doi: 10.1109/CVPR.2012.6247817

### Competition Sponsor
- **Organization:** Cornell University
- **Address:** 373 Pine Tree Road, Ithaca, NY 14850

---

*This document is intended for LLMs assisting with the MABe Challenge. It contains comprehensive information about the competition structure, data, rules, and successful approaches from other competitors.*

