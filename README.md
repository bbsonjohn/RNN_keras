# RNN_keras
A recurrent neural network for natural language processing and sequence generation using Keras framework.
We thank **deeplearning.ai** for inspiring the program.

In this program, we carry out two tasks with Recurrent Neural Network (RNN): 1.) translate French sentences to English sentences; 2.) generate original English sentences by sequence sampling.

In the first part, we use RNN with attention to translate French to English. We shows the two versions of the same sentence, both in words representation and indices representation.

<img src="figures/OriginalSent.png" width=100%>

<img src="figures/IndicesRep.png" width=100%>

This is the RNN model we train to translate sentences, with long-short term memory (LSTM) cell as the building blocks. First, the French sentence is decoded with a bidirectional LSTM. We pass the sequence output to attention blocks. The attention blocks allow an output sequence of length (Ty) different from that of the input sequence (Tx), and also each output word in the sequence has attention over the whole input sequence. Note that each input to the attention layer is distributed across all attention cells. The output sequence is encoded by a (unidirectional) LSTM with the contents of the attention blocks as the local input sequence.

<img src="figures/RNNGraph.png" width=100%>

In the following figure we also shows the structure of the attention block. Basically it is a layer of neurons discconected in the input time steps, and another layer of neurons fully-connected in the input time steps. The output activations become the weights for the words in the input sequence on the output sequence.

<img src="figures/attention.png" width=100%>

Once again, we train the model using Adam optimizer. In the sample file, we did not train the parameters to the model full potential to save the computation time, i.e. the loss is still steadily dropping towards the final epoches. 

<img src="figures/train.png" width=100%>

<img src="figures/loss.png" width=40%>

<hr>

On the second part of this file, we train the network with the relatively simple RNN model, i.e. a unidirectional LSTM RNN with the same length between input and output sequences, Tx = Ty. We train the model by asking the model to predict the next word of a sentence given the previous word, and optimize the accuracy of the prediction.

<img src="figures/RNNCell.png" width=30%>

After training the parameters, we restructure the RNN such that it generates new sentences. We do so by the model below. We fit the output word of an RNN cell to the input of the next RNN cell. i.e. we ask the RNN to predict a new word at t based on its own prediction at t-1.  We initialize the sequence at t = 0 by some arbitrary word or a null word. This way the RNN can generate a sentence itself without reliance on external example except at initialization.

<img src="figures/SeqSampling.png" width=80%>

The resultant sentence generated from this sequence sampling procedure.

<img src="figures/sample.png" width=100%>
