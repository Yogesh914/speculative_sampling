# 🤑 Direct Preference Optimization: Your Language Model is Secretly a Reward Model
#research/read[dp_optim.pdf](%F0%9F%A4%91%20Direct%20Preference%20Optimization%20Your%20Language%20Model%20is%20Secretly%20a%20Reward%20Model/dp_optim.pdf)<!-- {"embed":"true", "preview":"true"} -->
## Abstract
* The aim of the paper is to map between the reward function and optimal reward policies for optimizing model to align with preferences by a single stage policy training approach making it computationally efficient
* The paper introduces DPO (direct preference optimization) which is their proposal for a more efficient and direct procedure to RLHF

## Introduction
* Challenge Existing: 
  * LLMs have a broad knowledge but since it is difficult to control its generation behavior we have control mechanisms like RLHF to steer the generation as per preference but it is complex and unstable in nature
  * In more detail, training on a large variety of datasets with different goals, skillsets, and priorities, can result in some of them not being desirable. For eg: We need the model to know about common misconceptions among people but should be aware that it is a misconception
* Existing Solution: In simple terms, it is important to select model responses and have steering control over model generation capabilities to match our own preferences for which mechanisms like RLHF are used
  * RLHF: Reinforcement Learning with Human Feedback is a method based on RL which involves creating a reward model based on preference datasets and utilizing it to optimize the SFT model performance
  * Problems in RLHF: RLHF is very expensive and complex due to its involvement of multiple training loops

## Methodology
* DPO is a computationally efficient method that calculates the log probabilities of preferred and non-preferred completions under a model and optimizes its params in a way to increase the likelihood of preferred responses and decrease those non-preferred to align the model with human preferences
* Steps For DPO in comparison to RLHF
  * DPO:
    1. Take a base pre-trained model
    2. Use the base model to generate pairs of outputs in response to a variety of prompts. These outputs will be used to gauge which responses are more aligned with human preferences
    3. Adjust the parameters of the language model so that it assigns a higher probability to outputs than human raters preferred and a lower probability to less-preferred outputs, the model is also encouraged to adjust its distribution towards the preferred completions in relation to the original model to avoid reward hacking.
* RLHF:
  1. Take a base pre-trained model
  2. Use the base model to generate pairs of outputs in response to a variety of prompts. These outputs will be used to gauge which responses are more aligned with human preferences
  3. Compile human ratings on the generated pairs of outputs and use these ratings to train a reward model, this reward model learns to predict the quality or desirability of outputs based on human feedback, effectively translating diverse forms of feedback into a quantifiable reward signal
  4. Use reinforcement learning algorithms (PPO) to fine-tune the language model. This involves generating outputs, evaluating them with the reward model, and adjusting the model parameters to maximize the predicted rewards, the model is also encouraged to minimize the KL divergence between the current and old model to avoid reward hacking
* **DPO** integrates human preferences directly into the model's training process, encouraging it to reproduce outputs that align with those preferences. This is done by constructing a loss function that reflects the preferences shown in the human-labeled data.
* **RLHF** constructs an intermediate reward model that interprets human feedback and uses it as a signal for a subsequent reinforcement learning phase. This approach decouples the process of understanding human feedback from the process of generating better outputs according to that feedback.
