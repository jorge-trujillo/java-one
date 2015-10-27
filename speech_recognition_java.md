# Speech Recognition in Java

* Speech recognition has improved a lot recently, with more data, machine learning, better algorithms.
* Many open source libraries are available
  * Python: Theano, Caffe
  * Java: dl4j, H2O
* Open source data sets
* Still very complex, not accessible to new users.

### User Expectations

* Predictable
* Control interface is simple
* Recognition is fast and accurate
* In many cases, current issues with speech recognition have more to do what happens with the recognized text than the speech recognition itself

### Using Sphinx

* CMU Sphinx Recognition
* Training models is very time consuming (months)
* Can use pretrained models
```java
config.setAcousticModelPath()
```
* Need a dictionary to match phonemes
```java
config.setDictionaryPath("resource:<language>.dict");
```
* If you want to recognize general purpose speech, you need to train a langyage model
  * Can pass in all the text
  * Tools: Boilerpipe, imtool
* Grammaar model is used to reconize speech
```
config.setGrammarPath("Resource:<grammar>.gram");
```
* Next would use `LiveSpeechRecognizer`
* `StreamSpeechRecognizer` works based on files


### Improving Recognition

* Swap grammars
* Structure commands to reduce phonetic similarity
