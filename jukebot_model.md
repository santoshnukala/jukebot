
The purpose of this notebook is to show how the model was constructed and to describe more concretely how our implementation works.  We started and adapted our model from work done by Tyler Doll:  
https://towardsdatascience.com/making-music-with-machine-learning-908ff1b57636



```
import glob
import pickle
import numpy
import matplotlib.pyplot as plt 
from music21 import converter, instrument, note, chord
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.layers import LSTM
from keras.layers import Activation
from keras.layers import BatchNormalization as BatchNorm
from keras.utils import np_utils
from keras.callbacks import ModelCheckpoint

```

Above are the imports we utilized for our model.  Pickle allowed us to easy save as restore Python objects such as the weights files.  We used Matplotlib to plot the loss functions from training.  Keras is a high-level API that simplifies creating neural networks.  Music21 allowed us to parse the midi files and derive (note, octave) pairs easily.  


```
globalSequence = 100  #sequence length we will be considering
globalEpoch = 200     #number of epochs we will be training for, used as parameter in fit() function for network
epochList = list(range(1,globalEpoch+1))  #list solely for plotting
```

globalSequence and globalEpoch were variables we set in advance so that we would not have "magic numbers" referring to the sequence length and the number of epochs respectively


```
def obtain_notes():
    
    notes = []  #notes is where all (note, octave) pairs will be stored in the order they appear in the songs

    for midi_song in glob.glob("song_folder/*.mid"): #loop through all the midi files in the song_folder
        parsed_song = converter.parse(midi_song) 

        intermediate_notes = None   #pre-initialize intermediate_notes

        try: # file has instrument parts
            s2 = instrument.partitionByInstrument(parsed_song) #separate based on instrument
            intermediate_notes = s2.parts[0].recurse()  #get first instrument, we are focusing only on single instrument music, specifically piano
        except: # all the notes are uniform in the file without consideration for instrument
            intermediate_notes = parsed_song.flat.notes   #in this case no need to separate based on instrument

        for individual_note in intermediate_notes:
            if isinstance(individual_note, note.Note):  #if we see an individual note, then just add it
                notes.append(str(individual_note.pitch))  #the pitch refers to the note i.e A,B,C,...G and the octave
            elif isinstance(individual_note, chord.Chord):  #treat chords as a group of notes
                notes.append('.'.join(str(n) for n in individual_note.normalOrder))

    with open('data/notes', 'wb') as filepath:
        pickle.dump(notes, filepath) #put the notes at this file location for future use

    return notes
```

The purpose of the obtain_notes function is to iterate through every single one of the midi files in the specified folder and "extract" the necessary data from the midi files.  Music21 allowed us to first partition each file based on instrument.  For the sake of reducing model complexity, we decided to consider the first instrument only and compose our dataset of single-instrument songs (specifically piano).  Once we parse the individual song, we can iterate through the song and append the pitch to our notes list.  Once we have appended all the notes from each song to the list, we use pickle to store the list because we need to refer to it later.  


```
def prepare_sequences(notes, numClasses):
    """ Prepare the sequences used by the Neural Network """
    length = globalSequence


    # get all the (note, octave) pairs that occur in the training data.  These will be our class labels
    noteOctavePairs = sorted(set(item for item in notes))

     # Use a dictionary to create a functional relationship between (note, octave) and the integers
     #once they are integers we can use them for our neural network

    note_integer_function = dict((note, number) for number, note in enumerate(noteOctavePairs))

    predictors = []   #what we will feed into neural network
    responses = []   #the expected class response

    # create input sequences and the corresponding class response
    for i in range(0, len(notes) - length, 1):
        sequence_in = notes[i:i + length]  #look at this group of sequence_length notes
        sequence_out = notes[i + length]     #the (sequence_length + 1)nth note is the output
        predictors.append([note_integer_function[char] for char in sequence_in])
        responses.append(note_integer_function[sequence_out])

    n_patterns = len(predictors)

    # Ensure input is in shape suitable for LSTM layers
    predictors = numpy.reshape(predictors, (n_patterns, length, 1))
    # normalize input
    predictors = predictors / float(numClasses)

    responses = np_utils.to_categorical(responses)

    return (predictors, responses)

```

The main purpose of the prepare_sequences function above is reformat the (note, octave) pairs into something that is suitable for our neural network.  We first needed to map each unique (note,octave) pair to an integer which we accomplished using a dictionary.  Now that we were dealing with integers, we needed to determine what the network inputs and outputs would be.  Given the sequential nature of music, we iterated through our notes list and added each sequence of notes to our list of network inputs.  The next note that followed is our desired network_output.  Our function also takes in a parameter called numClasses which is the number of possible outputs we have (number of unique (note,octave) pairs).  We return the network input and output since these are used later in setting up the network.  


```
def create_network(predictors, num_class_labels):
    
    dropoutRate = 0.3
    LSTM_nodes = 512
    Dense_nodes = 256     #parameters determined through trial and error

    network = Sequential()
    network.add(LSTM(
        512,
        input_shape=(predictors.shape[1], predictors.shape[2]),
        recurrent_dropout=dropoutRate,
        return_sequences=True
    ))
    network.add(LSTM(LSTM_nodes, return_sequences=True, recurrent_dropout=dropoutRate,))
    network.add(LSTM(LSTM_nodes))
    network.add(BatchNorm())  
    network.add(Dropout(dropoutRate))
    network.add(Dense(Dense_nodes))
    network.add(Activation('relu'))   #activation layer described below
    network.add(BatchNorm())
    network.add(Dropout(dropoutRate))
    network.add(Dense(num_class_labels))     #layer for number of class labels
    network.add(Activation('softmax'))  #output probabilities for each class label
    network.compile(loss='categorical_crossentropy', optimizer='rmsprop')

    return network
```

In our create_network function, we can finally start preparing the neural network to use the inputs and outputs we have derived earlier.  Frankly, in terms of determining which layers we wanted to use, it was a lot of guessing and checking.  We would have liked to use a more rigorous form of comparison such as cross-validation, but each time we tuned the parameters, it took 12-24 hours just to get a result.  If someone's internet dropped while training the network, all progress was lost since Google Colab does not save files between runtime sessions.  In short, the structure of the network was largely based on experimentation and research we found online.  

The purpose of the LSTM layers in the model are to use the memory gates and forget gates to tackle the sequential nature of this model.  The Dropout layers allow us to ignore a certain portion of the previous layer's outputs which prevents overfitting.  We played around with the proportion of outputs being ignored and found the best results at 30% (.3 in the code above).  The dense layers function like a typical layer in basic neural network in that they are just using weights to feed inputs forward deeper into the network.  The 'relu' activation layer serves to ignore inputs that are less than 1; it is also useful because the function is piecewise linear.  This allows for nice differentiability properties which is useful during back-propagation.  The last softmax activation layer allows us to determine which (note, octave) pair will be predicted based on the previous layer's outputs.  It is an extension of the logistic function which has nice differentiability properties. We also used BatchNormalization layers to ensure that the different inputs and outputs were scaled properly.  For a more thorough explanation of the theory behind these layers, see our paper.   

To calculate the loss, we decided to use crosscategorical entropy since this is widely used in multiclass classification problems.  


```
def train(network, predictors, responses):
    """ train the neural network """
    
    filepath = "weights.hdf5"
   

    checkpoint = ModelCheckpoint(
        filepath,
        monitor='loss',
        verbose=0,
        save_best_only=True,
        mode='min'
    )
    callbacks_list = [checkpoint]
    #use callbacks to get model status each epoch

    
    theModel = network.fit(predictors, responses, epochs=globalEpoch, batch_size=128, callbacks=callbacks_list)
    #train model for specific number of epochs

    #plotting loss, from the model after training
    plt.plot(epochList, theModel.history['loss'])
    plt.title('model loss')
    plt.ylabel('loss')
    plt.xlabel('epoch')
    plt.savefig('loss.png')
```

Here we use fit() to train the network for a specified number of epochs.  After each epoch, it will generate a weights file which can be viewed as the model's progress at that time in training.  We assign the model to a variable so we can access the loss function and plot it accordingly. Ideally we would have tuned some of the parameters such as batch_size, but as mentioned above there were other factors we wanted to test in isolation.   


```
def train_model():
    
    notes = obtain_notes()    #call the obtain_notes function

    # get number of distinct (note,octave) pairs
    numClasses = len(set(notes))

    predictors, responses = prepare_sequences(notes, numClasses)  #get inputs and outputs for neural network

    network = create_network(predictors, numClasses)  #create network

    train(network, predictors, responses) #keep training for specific number of epochs
    
if __name__ == '__main__':
    train_model()
```

In the train function we call all of the functions we have defined above.  Finally, we call the train_network function which executes all of the above code.  Below, we show how we generated midi files from the trained model.  


```
import pickle
import numpy
from music21 import instrument, note, stream, chord
from keras.models import Sequential
from keras.layers import Dense
from keras.layers import Dropout
from keras.layers import LSTM
from keras.layers import BatchNormalization as BatchNorm
from keras.layers import Activation
```

We need to use the same imports since we are reinitializing the network with the weights we obtained from training (originally we had these as 2 separate files).  


```
def prepare_sequences(notes, pitchnames, numClasses):
    
    # functional mapping of (note,octave) to integer using dictionary
    note_integer_function = dict((note, number) for number, note in enumerate(pitchnames))

    length = globalSequence
    predictors = []
    responses = []
    for i in range(0, len(notes) - length, 1):
        input = notes[i:i + length]     #predict using x notes
        response = notes[i + length]      #guess x+1st note
        predictors.append([note_integer_function[element] for element in input])  
        responses.append(note_integer_function[response])

    n_patterns = len(predictors)

    # Ensure input is in shape suitable for LSTM layers
    normalized_predictors = numpy.reshape(predictors, (n_patterns, length, 1))
    # normalize
    normalized_predictors = normalized_predictors / float(numClasses)

    return (predictors, normalized_predictors)
```

Similar to what was done above, we want to prepare sequences to use as the network input.  We do not end up using anything close to the true amount of sequences; we are just going to use a random point in the network input to generate new output later on.  


```
def create_network(network_input, numClasses):
    LSTM_nodes = 512
    Dense_nodes = 256
    dropout_rate = 0.3

    network = Sequential()
    network.add(LSTM(
        LSTM_nodes,
        input_shape=(network_input.shape[1], network_input.shape[2]),
        recurrent_dropout=dropout_rate,
        return_sequences=True
    ))
    network.add(LSTM(LSTM_nodes, return_sequences=True, recurrent_dropout=dropout_rate,))
    network.add(LSTM(LSTM_nodes))
    network.add(BatchNorm())
    network.add(Dropout(dropout_rate))
    network.add(Dense(Dense_nodes))
    network.add(Activation('relu'))
    network.add(BatchNorm())
    network.add(Dropout(dropout_rate))
    network.add(Dense(numClasses))
    network.add(Activation('softmax'))
    network.compile(loss='categorical_crossentropy', optimizer='rmsprop')

    # Load the weights to each node
    network.load_weights('weights.hdf5')

    return network
```

In this create_network function we are just recreating the exact same network we  did from training.  The only main difference is that we are loading the weights that we obtained from training so we can now make predictions.  


```
def generate_notes(network, network_input, pitchnames, numClasses):
    """ Generate notes from the neural network based on a sequence of notes """
    # pick a random sequence from the input as a starting point for the prediction
    start = numpy.random.randint(0, len(network_input)-1)

    note_integer_function = dict((number, note) for number, note in enumerate(pitchnames))

    pattern = network_input[start]
    prediction_output = []

    #make 500 predictions for 500 notes
    for note_index in range(500):
        prediction_input = numpy.reshape(pattern, (1, len(pattern), 1))
        prediction_input = prediction_input / float(numClasses)

        prediction = network.predict(prediction_input, verbose=0) #make prediction using random sequence from above

        index = numpy.argmax(prediction)    #from softmax get maximum probability which corresponds to prediction
        result = note_integer_function[index]   #convert result to (note,octave)
        prediction_output.append(result)        #append to output list

        pattern.append(index)         #append to original random sequence
        pattern = pattern[1:len(pattern)]     #consider 100 most recent notes

    return prediction_output    #return list of (note, octave)
```

In our generate_notes function, we can finally predict (note,octave) pairs with our neural network.  We accomplish this by choosing a random index from our network inputs which corresponds to a sequence of notes we will input into our network.  Afterwards, we use our network to predict what would come next.  This prediction is appended to an array which we return.  It is also appended to our network input array.  For the next prediction, we consider the most recent notes in the network input array (this excludes one of the old notes and includes one of the predicted notes).  We repeated this 500 times to generate roughly 90 seconds of music.  Afterwards we return the array containing all of our predicted notes.  


```
def create_midi(output_sequence):
    """ convert the output from the prediction to notes and create a midi file
        from the notes """
    noteSeparation = 0
    output_notes = []

    # create note/chords for midi file depending on output sequence
    for pattern in output_sequence:
        # check for chord
        if ('.' in pattern) or pattern.isdigit():
            notes_in_chord = pattern.split('.')
            chordList = []    #create chord from individual notes
            for current_note in notes_in_chord:
                new_note = note.Note(int(current_note))
                new_note.storedInstrument = instrument.Piano()  #assume instrument is piano
                chordList.append(new_note)    #add note to chord
            new_chord = chord.Chord(chordList)
            new_chord.offset = noteSeparation
            output_notes.append(new_chord)    #append to final output
        # check for note
        else:
            new_note = note.Note(pattern)
            new_note.offset = noteSeparation
            new_note.storedInstrument = instrument.Piano()    #assume instrument is piano
            output_notes.append(new_note)     #append to final output

        # increase offset each iteration so that notes do not stack
        noteSeparation += 0.5

    midi_stream = stream.Stream(output_notes)

    midi_stream.write('midi', fp='output.mid')

```

Using the predictions generated in the previous function, we now need to convert these to a midi file.  Once again, we use Music21 to accomplish this.  We assumed a constant distance between each note/chord for simplicity's sake.  If we included varied offsets from the beginning, it would increase the number of classes we are trying to predict dramatically.  This is because we would be trying to predict a (note, octave, offset) tuple rather than a (note, octave) pair.  



```
def generate():
    """ Generate a piano midi file """
    #load the notes used to train the model
    with open('data/notes', 'rb') as filepath:
        notes = pickle.load(filepath)

    # Get all pitch names
    pitchnames = sorted(set(item for item in notes))
    # Get total number of classes
    numClasses = len(set(notes))

    #run functions defined above
    network_input, normalized_input = prepare_sequences(notes, pitchnames, numClasses)
    model = create_network(normalized_input, numClasses)
    prediction_output = generate_notes(model, network_input, pitchnames, numClasses)
    create_midi(prediction_output)
if __name__ == '__main__':
    generate()
```

In the generate() function, we run the functions described and their intermediate outputs to output a midi file.  
