# Overview of Project
1. Calculation of emission probabilities e(x|y)
2. Calculation of transition probabilties q(x|y)
3. Using emission probabilities to implement a simple sentiment analysis system
4. Implementation of basic viterbi algorithm
5. Implementation of modified viterbi algorithm to find k-best sequences
6. Use of laplace smoothing on emission probabilities to improve results

Given: labelled training and test sets that consists of word tokens and their corresponding labels (sentiments)

```
disfrutemos O
de O
una O
buenisima O
calidad O
en O
el O
producto B-positive
...
```

Test results and further explanations can be found in the "ML Project Report" pdf

# Part 1: Emission Probabilities

## Emission Probability e(x|y)
Emission Probability e(x|y) is the probability of generating observation x from state y and is calculated as follows:
count(y->x)/count(y) where 
count(y->x): Number of times we see state y generated from observation x
count(y): Number of times we see state y 

1.1) Function that estimates emission parameters from the training set using Maximum Likelihood Estimation (MLE)

1.2) Function that estimates emission parameters from the training set using Maximum Likelihood Estimation (MLE), accounting for the possibility that words that do not appear in the training set

1.3) Implement a simple sentiment analysis system on a test set that uses 1.2 (generate the set of y's that produces the highest emission probability for x using the training set)

# Part 2: Transition Probabilities and Viterbi Algorithm

## Transition Probability q(y_i|y_i-1)
Transition Probability q(y_i|y_i-1) is the probability of generating next state y_i from state y_i-1 and is calculated as follows:
count(y_i-1 -> y_i)/count(y_i-1) where 
count(y_i-1 -> y_i): Number of times we see a transition from state y_i-1 to state y
count(y): Number of times we see state y 

## Viterbi Algorithm 

### Initialisation
1. Initialise a score matrix of all 0s to store the highest scoring paths from the START state to state u at position j. The dimensions of the score matrix is num_positions x num_states

2. Assign value 1 to the START state

### Forward Pass
```
For each position in the sentence till the 3rd last position:
    For each u (except START and STOP):
        For all v, calculate:
        score of v in position j * emission prob of observation o/UNK from u in position j+1 * transition prob of v to u
        Store the highest score in score_matrix[position j+1][u]

For the position at the STOP state:
    For each v(except START and STOP), calculate:
    score of v in 2nd last position * transition prob of v to STOP state
    Store the highest score in score_matrix[position of STOP][STOP state]
        

By performing the forward pass, we are storing the highest scoring path from START state to the current state,
taking into account the score of all previous state, the transition probabilities and the current emission probability 
```

### Backwards Pass
Initialise a list of empty labels with the same length of the sentence

```
For the position before STOP state:
    For each u (except START and STOP):
        Calculate: score of u * transition prob of u to STOP state
        Store the highest state in the empty list at the correct position

From the second last word (third last position) to the first word (second position):
    For each u (except START and STOP):
        Calculate: score of u * transition prob of u to optimal state found at the next position
        Store the highest state in the empty list at the correct position
```

By performing the backwards pass, we can find the overall highest scoring path, that is the path with the highest multiplication of scores and transitions

2.1) Function that estimates transition parameters from the training set using Maximum Likelihood Estimation (MLE) 


2.2) Learn the model parameters (estimated emission and transition probabilities from the training set) and implement the Viterbi algorithm on the test set

# Part 3: Designing an Algorithm to find the k-th best output sequence

3) Here, we implented a Modified Viterbi Algorithm to compute k-best sequences using beam search in the forward pass, keeping only the k-best sequences after each position has been iterated through. More details can be found in the "ML Project Report"

# Part 4: Improving results

4) Here, we improved the results by using laplace smoothing to better handle the problem of emission probability for unknown words. More details can be found in the "ML Project Report"