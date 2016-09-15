# Wavenet implementation
Based on https://deepmind.com/blog/wavenet-generative-model-raw-audio/ and https://arxiv.org/pdf/1609.03499.pdf.
Disclaimer: this is a re-implementation of the model described in the WaveNet paper by Google Deepmind. This repository is not associated with Google Deepmind.

## Installation:
- `pip install -R requirements.txt`

## Training:
```$ python wavenet.py```

Or for a smaller network (less channels per layer).
```$ python wavenet.py with small```

### Options:
Train with different configurations:
```$ python wavenet.py with 'option=value' 'option2=value'```
Available options:
```
 batch_size = 64
  data_dir = 'data'
  debug = False
  desired_sample_rate = 4410
  dilation_depth = 9
  early_stopping_patience = 20
  fragment_length = 1024
  fragment_stride = 2045
  keras_verbose = 1
  learn_all_outputs = True
  nb_epoch = 1000
  nb_filters = 256
  nb_output_bins = 256
  nb_stacks = 1
  run_dir = None
  seed = 3004083
  train_only_in_receptive_field = True
  use_bias = False
  use_skip_connections = True
  use_ulaw = True
  optimizer:
    decay = 0.0
    epsilon = None
    lr = 0.001
    momentum = 0.9
    nesterov = True
    optimizer = 'sgd'
```

## Using your own training data:
- Create a new data directory with a train and test folder in it. All wave files in these folders will be used as data.
    - Caveat: Make sure your wav files are supported by scipy.io.wavefile.read(): e.g. don't use 24bit wav and remove meta info.
- Run with: `$ python wavenet.py 'data_dir=your_data_dir_name'`

## Sampling:
Once the first model checkpoint is created, you can start sampling.

Run:
```$ python wavenet.py predict with /Users/bas/projects/wavenet/models/run_2016-09-14_11:32:09/config.json predict_seconds=1```

The latest model checkpoint will be retrieved and used to sample. The sample will be streamed to `[run_folder]/samples`, you can start listening when the first sample is generated.

### Sampling options:
- `predict_seconds`: float. Number of seconds to sample.
- `sample_argmax`: `True` or `False`. Always take the argmax
- `sample_temperature`: `None` or float. Controls the sampling temperature. 0.01 seems to be a good value.
- `seed`: int: Controls the seed for the sampling procedure.
e.g.:
```$ python wavenet.py predict with models/[run_folder]/config.json predict_seconds=1 sampling_temperature=0.1```