# 🎛️ Accelerating Large Language Model Decoding with Speculative Sampling
#research/read[llm_decoding.pdf](README/llm_decoding.pdf)<!-- {"embed":"true", "preview":"true"} -->
## Abstract
* The paper introduces the idea of a sampling algorithm called speculative sampling, which according to their results speeds up the LLM decoding process by 2-2.5 times the speed, without compromising the accuracy
* The basic idea is running two models in parallel, one smaller model that is faster, called the **draft model**, and a bigger model called the **target model**
* The draft model is used to generate the “easier” tokens whereas the target model fills in the rest (on the parts it disagrees with the draft model)

## Introduction
* Typically the time taken for transformer models to generate a single token is proportional to the number of model parameters
* Since the next token predicted depends on the previous, which requires the transformer to to do a full forward pass over the prior context every time, which takes more time for larger models with more parameters.
* The proposed algorithm, preserves accuracy whilst speeding up this decoding process

## Methodology
* **Setup**: You have two models, a main one (q) that's the target model, and a smaller secondary one (p) that's the draft model.
* **Predict in advance**: You think ahead (lookahead K) and create several (K) tokens using the auto-regressive draft model, by providing it the prefix 
* **Check each prediction**: You then use your target model on the prefix + the draft model generations to evaluate each of them, and producing probability distributions for each of the K tokens generated, at the same time generate a K+1 token in case needed later
* **Decision Time (Rejection Sampling)**:  
  * Case 1: If the draft model's word is good enough (meaning its probability is not worse than the final model's word), you accept this draft word
  * Case 2: If it's not good enough, i.e. q(x) < p(x) then using a sample from uniform distribution we determine a threshold for deciding whether or not to accept it or not using q(x)/p(x)
    * Case 2.5: If rejected in this case then, we stop the loop process and sample from a adjusted distribution: (q(x) - p(x))+  
* **Loop Until Complete**: As long as you don’t run into Case 2.5 (i.e. reject the draft model prediction), you keep doing this until you've built a sentence that's as long as you wanted it to be
* The best case scenario for the algorithm is that it will generate K+1 tokens, worst case is 1 token
* In essence, this algorithm tries to 'speculate' or predict a few words ahead and checks if these predictions are good enough to keep. If they are not, it corrects them on the spot.


## Related Work

## Results

