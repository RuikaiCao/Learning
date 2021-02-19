# Useful Resources

- https://classroom.udacity.com/nanodegrees/nd892-ent/dashboard/overview

## Books
- http://aima.cs.berkeley.edu/
- https://home.cs.colorado.edu/~martin/SLP/

## Papers
- [PoS Tagset](https://arxiv.org/pdf/1104.2086.pdf)
- [Stats Language Models](https://www.cs.cmu.edu/~roni/papers/survey-slm-IEEE-PROC-0004.pdf)
- [LDA](https://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)
- [RNN](https://onlinelibrary.wiley.com/doi/epdf/10.1207/s15516709cog1402_1)
- [RNN Recommender](https://arxiv.org/pdf/1511.06939.pdf)
- [LSTM](http://www.bioinf.jku.at/publications/older/2604.pdf)
- [Gradient Clipping](https://arxiv.org/abs/1211.5063)

## Knowledge
- [Penn Treebank PoS](https://www.ling.upenn.edu/courses/Fall_2003/ling001/penn_treebank_pos.html)
- [Brown Corpus](https://en.wikipedia.org/wiki/Brown_Corpus)
- [Spacy: lingustic features](https://spacy.io/usage/linguistic-features)
- [Wikipedia: Forward Algorithm](https://en.wikipedia.org/wiki/Forward_algorithm)
- [Wikipedia: Viterbi Algorithm](https://en.wikipedia.org/wiki/Viterbi_algorithm)
- [Wikipedia: Vanishing Gradient](https://en.wikipedia.org/wiki/Vanishing_gradient_problem)
- [Wikipedia: Time Delay NN](https://en.wikipedia.org/wiki/Time_delay_neural_network)
- [Wikipedia: RNN](https://en.wikipedia.org/wiki/Recurrent_neural_network#Elman_networks_and_Jordan_networks)
- [Backpropagation](https://www.ics.uci.edu/~pjsadows/notes.pdf)
- [Calculus Rules](http://www.columbia.edu/itc/sipa/math/calc_rules_multivar.html)
- [Common Derivatives Integrals](https://tutorial.math.lamar.edu/pdf/Common_Derivatives_Integrals.pdf)
- [Learning Rate in Gradient Descent](http://blog.datumbox.com/tuning-the-learning-rate-in-gradient-descent/)
- [CS321n: CNN for Vision Recognition](https://cs231n.github.io/neural-networks-3/)
- [Andrej Karpathy's lecture on RNNs and LSTMs from CS231n](https://www.youtube.com/watch?v=iX5V1WpxxkY)
- [Edwin Chen's LSTM post](http://blog.echen.me/2017/05/30/exploring-lstms/)
- [Chris Olah's LSTM post](http://colah.github.io/posts/2015-08-Understanding-LSTMs/)
- [LSTM Tutorial](https://web.archive.org/web/20190106151528/https://skymind.ai/wiki/lstm)
- [GRU](http://www.cs.toronto.edu/~guerzhoy/321/lec/W09/rnn_gated.pdf)
- [Wikipedia: Zipf's Law](https://en.wikipedia.org/wiki/Zipf%27s_law)

## Softwares
- https://pomegranate.readthedocs.io/en/latest/index.html
- https://www.nltk.org/
- https://radimrehurek.com/gensim/index.html

## Usecases
- https://engineering.fb.com/2016/10/25/ml-applications/building-an-efficient-neural-language-model-over-a-billion-words/
- https://aws.amazon.com/lex/faqs/
- http://www.cs.toronto.edu/~graves/handwriting.cgi?text=My+name+is+Luka&style=&bias=0.15&samples=3
- https://www.youtube.com/watch?v=0FW99AQmMc8
- https://openai.com/blog/dota-2/

## Data
- http://ai.stanford.edu/~amaas/data/sentiment/


# Part 1 - Introduction

## Lesson 3 - Intro to NLP

1. NLP is a difficult task because natrual languages are not as structured as mathematical or logical languages.
2. Computers can complete such tasks to a certain level.
    1. process words & phrases
        - keywords
        - part of speech
        - named identities
        - dates & quantities
    2. parse sentences
        - statements
        - questions
        - instructions
    3. analyze Documents
        - frequent & rare words
        - tone & sentiment
        - documents clustering
3. Different contexts give same sentences different meanings, which is difficult for computers to understand.
4. NLP pipelines
    1. **text processing**: obtain & clean plain texts
    2. **feature extraction**: convert texts to numerics
    3. **modeling**: obtain predictions

## Lesson 4 - Text Processing

1. Clean raw data to obtain plain text (e.g. remove html tags).
2. normalization
    - case normalization
    - puncuation removal
    - tokenization: splitting text into tokens (words)
    - stop words removal
    - part of speech tagging
    - named indentity recognition
    - stemming & lemmatization: reduce words to its root
        - stemming: simple, brute rules like removing "ing"
        - lemmatization: using a dictionary to map
  
## Lesson 5 - Spam Classifier with Naive Bayes

1. Bayes Theorem
2. Naive Bayes Algorithm
    1. Assuming B1, B2, etc. are independent, then P(A | B1, B2) is proportional to PP(A) = P(B1|A)\*P(B2|A)\*P(A).
    2. Compute PP(A) and PP(not A)
    3. Normalize PP(A) & PP(not A) so that P(A)+P(not A)=1

## Lesson 6 - Part of Speech Tagging with the Hidden Markov Model (HMM)

1. part of speech (PoS): noun, verb, model verb, adjective, adverb, etc.
2. look up table
    - row: word
    - column: PoS
    - value: frequency in training data
3. bigram look up table
    - row: words bigram
    - column: PoS bigram
    - value: frequency in training data
4. problem of bigram look up table: bigram missing in training data
5. Hidden Markov Model (HMM)
    1. z = (A, B) is a HMM
    2. A: emission probability distribution, i.e. P[Y(t) | X(t)]
    3. B: state transition probability distribution, i.e. P[X(t) | X(t-1)], P[X(0)]
    4. At each time t, X(t) is the hidden state, Y(t) is an observation
    5. likelihood estimation: given z=(A,B) and a set of observation Y, determine P[Y|z]
        - forward algorithm
    6. hidden state decoding: given z={A,B} and a set of observation Y, determine Q, the most likely sequence of hidden states
        - Viterbi algorithm: for each hidden state, find the path that ends with it with highest probability.
    7. Parameter learning: given a model topography (set of states and connections) and a set of observations Y, learning the transition probability A and emission probability B
6. HMM used in PoS tagging
    1. hidden state: part of speech
    2. observations: terms (words)
    3. emission probability: P(term | PoS)
    4. transition probability: P(next PoS | current PoS). Starts and ends of sentences are also considered as PoS.
    5. HMM: combining transition probability among hidden states (PoS) and emission probability between hidden states and observations (terms)
    6. path: connected hidden states with transition probability on edges and emission probability on vertices
    7. estimation: choose the tagging with highest probability along the path

# Part 2 Computing with Natural Language

## Lesson 1 - Feature Extraction and Embeddings

### Bag of Words

- corpus: collection of documents
- vocabulary: collection of unique words in corpus
- document-term matrix
    - columns: terms in vocabulary in some order. Each column is a term
    - rows: each document is a row
    - values: term frequency in the document, or P(t|d) - probability of term occurence in each document
- compare documents (rows)
    - dot product between each row = a . b: a measure of similarity between documents with a flaw: since only overlapping terms contribute to the calculation, both similar and distinct pairs of documents may have same dot product.
    - cosine similarity = a . b / ||a||.||b||: cosine of the angle between the two vectors (rows)

### TF-IDF
- purpose: to assign weights to different term in each documents
- term frequency (TF) tf(t,d) = count(t,d) / |d|
- inverse document frequency (IDF) idf(t,D) = log(|D|+1 / {count of doc containing t})

### One Hot Encoding
- transform each term into a vector with only one element as 1, all others 0
- disadvantage: create too many dimensions since there are a lot of terms

### Word2Vec
- continuous bag of words (CBoW): use neighboring terms to predict the middle term
- continuous skip-gram: use middle terms to predict neighboring terms
    1. Convert any term into an one-hot encoded vector
    2. Use the vector to train a neural network to predict neighboring terms
- Vector size is independent of the vocabulary.
- Train once, stored in a lookup table

### Glove
- co-occurence matrix: P(i|j) where term i co-occured (adjacent or close) with j
- Factorize the co-occurence matrix into a context matrix (terms as context) and a target matrix (target term)

### t-SNE
- t distributed stochastic neighbor embedding
- a way to visualize high dimentional space

## Topic Modeling

- bag of words: P(t|d) - probability of term occurence in each document
- Latent Variable: add a topic (z) layer between document and term: P(t|z) and P(z|d)
- Matices: the matrix of P(t|d) equals the multiplication of matix P(t|z) and P(z|d)

### Latent Dirichlet Allocation
- n topics, m terms
- Assumptions
    - P(t|z) follows a Dirichlet distribution with n parameters
    - P(z|d) follows a Dirichlet distribution with m parameters
- Algorithm
    - Initialize distributions of P(t|z) and P(z|d).
    - For each document in the training set, calculate the topic distribution.
    - Simulate a topic sequence of length l from P(z|d) for each document. l is a parameter with a Poisson distribution.
    - For each topic in a simulated sequence, simulate a term from P(t|z).
    - As a result, for each document, a fake document was simulated.
    - Calculate the error of the similarity between fake documents and true documents.
    - Adjust the parameters to move towards maximum similarity (how?)
    

# Extracurricular

## Feedforward Neural Network
- output Y = F(x, W)
- static mapping: fixing x and W, Y is fixed.
- training
    - feedforward: given x and W, calculate predicted output and error
        - for each layer h(p) = phi(h(p-1).W(p-1))
        - bias input: 1 as a constant input
    - backpropagation: update W to minimize the error
        - for each group of weights W[t] = W[t-1] + alpha * (- gradient of error w.r.t W)
        - for weight other than the last layer weights, use chain rule to compute gradient
- activation function phi: allow nonlinear relationship between inputs and outputs
    - hyperbolic tangent function: [-1,1]
    - sigmoid function: [0,1]
    - ReLU function: [0,1]
- regularization: dropout
- mini batch training: updating the weights every N steps by using the means of all N deltas

## RNN

- Elman Network (Simple RNN): include output of hidden layer from previous time point as input neurons

- backpropagation through time (BPTT)
    - update W_s - the weight matrix between hidden states: accumulative derivatives
    - update W_x - the weight between input and states: accumulative derivatives

## LSTM

- LTM: long term memory

- STM: short term memory

- E: event/observation

- learn gate
    - input: STM, E
    - output: N * i where N = tanh(W(STM, E)), i = sigmoid(W(STM, E))

- forget gate
    - input: LTM, STM, E
    - output: f * LTM where f = sigmoid(W(STM, E))

- remember gate
    - input: outputs of learn & forget gate
    - output: new LTM = summation of outputs from learn & forget gate

- use gate
    - input outputs from learn & forget gate
    - output: new STM = U * V where U = tanh(W(out of forget gate)) and V = sigmoid(W(output of learn gate, E))
    