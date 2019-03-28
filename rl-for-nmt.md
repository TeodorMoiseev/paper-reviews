## A Study of Reinforcement Learning for Neural Machine Translation
link: https://arxiv.org/pdf/1808.08866.pdf

-----

### Problem
Recent studies have shown that reinforcement learning is an effective approach for improving the performance of neural machine translation system. But successfully RL training is challenging due to its instability, especially in real-world systems with large datasets. This paper provides a systematic study of different RL techniques in application to NMT.

### Motivation to use RL in NMT
In usual training setup for NMT task, we optimize maximum likelihood of each produced token, but during inference we want to get realistic sentences, not single tokens. Using RL we can directly optimize evaluation metric (BLEU) which measures similarity between whole sentences instead of maximum likelihood for single tokens.

### MLE and RL Objective in NMT
#### MLE Objective
<img src="https://i.imgur.com/iby2SSw.png">

#### RL Objective
<img src="https://i.imgur.com/iBEz5RQ.png">

### Investigated methods
#### Sample generating
Two strategies are considered:
1. Beam search (the k most likely hypotheses are maintained during the inference)
2. Multinomial sampling (produces each word one by one through multinomial sampling over the model’s output distribution.)

#### Reward computation
There can be 2 main methods for reward calculation in NMT task:
1. Terminal: reward (BLEU with correct translation) is calculated when whole sentence in translated. This can lead to problems due to the sparsity of rewards.
2. Reward shaping: terminal reward is replaced by the sequence of intermediate rewards which is calculated as difference in partial BLEU: <img src="https://i.imgur.com/0JDfXz7.png" height="25">. It is verified that using the shaped reward instead of awarding the whole score does not change the optimal policy.
  
#### Variance reduction
Authors use REINFORCE algorithm for gradient estimation. They investigated two version of it:
1. Pure REINFORCE
2. REINFORCE with baseline

#### Combine MLE and RL Objectives
We can combine MLE and RL objectives linearly as follows: <img src="https://i.imgur.com/HDV0XC5.png" height="30">
  
#### Leveraging monolingual data
Monolingual data has been proved to be able to significantly improve the performance of NMT systems. There can be few options of how to use it in RL training:
##### With source-side monolingual data
We can use the NMT model trained from the bilingual data to beam search a target sentence and treat it as the pseudo target reference `y`. Afterwards `y'` is obtained via multinomial sampling to calculate the reward. The combination of beam search (to get the pseudo target reference sentence) and the multinomial sampling (to generate the action sequence of the agent) achieves good exploration-exploitation trade-off.

##### With target-side monolingual data
For target-side monolingual data authors get pseudo source via back translation. They first train a reverse NMT model from the target language to the source language with bilingual data. For each target-side monolingual sentence, using the reverse NMT model, they back translate it to get its pseudo source sentence.

#####  With both Source-Side and Target-Side Monolingual Data
1. Sequential approach: sequentially leverages source-side and target-side monolingual data for RL training.
2. Unified approach: genuine paired data, source monolingual data with pseudo target and target monolingual data with bacl translated source are packed together and treated as normal bilingual data.

### Results
#### Summary
1. Multinomial sampling is better than beam search for RL training
2. Reward shaping and baseline rewards does not make significant difference
3. The combination of the MLE and RL objectives is important
4. Leveraging the monolingual data can significantly improve quality of RL training

#### Tables
<img src="https://i.imgur.com/zgWSZYG.png" width="400"> <img src="https://i.imgur.com/yvqv4zX.png" width="400">
<img src="https://i.imgur.com/PSpbqWi.png" width="400"> <img src="https://i.imgur.com/PmyHBNe.png" width="400">
<img src="https://i.imgur.com/pnfvimb.png" width="400"> <img src="https://i.imgur.com/xUxFk71.png" width="400">
<img src="https://i.imgur.com/CQs9QaL.png" width="400"> 
<img src="https://i.imgur.com/pP7da3H.png">
