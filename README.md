# Enhancement-Coded-Speech

Please find the scripts referring to the paper "Convolutional Neural Networks to Enhance Coded Speech". We here provide the cepstral domain approach with the frame structure III. 

The code was written by Ziyue Zhao and Huijun Liu. 

## Introduction

A approach based on a convolutional neural network (CNN) is proposed to enhance coded (i.e., encoded and decoded) speech with the utilization of the cepstral domain features. The enhanced coded speech can achieve improved quality without modifing the codec (i.e., encoder and decoder) itself.

## Prerequisites

- [Python](https://www.python.org/) 2 or 3
- [Anaconda](https://anaconda.org/anaconda/python)
- CPU or NVIDIA GPU + [CUDA CuDNN](https://developer.nvidia.com/cudnn)

## Getting Started

### Installation

- Install [TensorFlow](https://www.tensorflow.org/) and [Keras](https://www.tensorflow.org/)
- Install [Matlab](https://www.mathworks.com/)

### Testing with the provided CNN model

- An example of the G.711-coded speech is included in the `dataset` folder 
- Run the Matlab script to prepare the input data for the CNN model, with G.711-coded speech sample `./dataset/exapmle_s_g711_coded.raw` and the means and standard variances from the training data `./mean_std_of_TrainData_g711_example.mat`, outputting the CNN input data `./type_3_cnn_input_ceps_v73.mat`, residual cepstral coefficients `./type_3_ceps_resi.mat`, and the phase angel vector `./type_3_pha_ang.mat`:
```bash
matlab Test_InputPrepare.m
```
- Run the Python script to use the CNN model, with the CNN input data `./type_3_cnn_input_ceps_v73.mat` and the provided CNN model `./cnn_weights_ceps_g711_best`, outputting the CNN output data `./type_3_cnn_output_ceps.mat`:
```bash
python CepsDomCNN_Test.py
```
- Run the Matlab script to obtain the final enhanced speech, with the CNN output data `./type_3_cnn_output_ceps.mat`, residual cepstral coefficients `./type_3_ceps_resi.mat`, the phase angel vector `./type_3_pha_ang.mat`, and G.711-coded speech sample `./exapmle_s_g711_coded.raw`, outputting the enhanced speech waveform `./cnn_postprocessed_8K_out.raw`:
```bash
matlab Test_WaveformRecons.m
```
### Reproduce the results

The results reported in the paper is tested on the NTT wideband speech database, so if you want to reproduce the exact results, the test need to be done with the same speech data (see details in the paper). 

### Training with your own dataset

- Run the Matlab script to prepare the CNN training data, with the uncoded speech for training `./dataset/example_uncoded_train_s.raw`, uncoded speech for validation `./dataset/example_uncoded_valid_s.raw`, coded speech for training `./dataset/example_coded_train_s.raw`, and coded speech for validation `./dataset/example_coded_valid_s.raw`, outputting training input `./Train_inputSet_g711.mat`, training target `./Train_targetSet_g711.mat`,  validation input `./Validation_inputSet_g711.mat`, and validation target `./Validation_targetSet_g711.mat`:
```bash
matlab Training_Data.m
```
- Run the Python scripts to train the CNN model, with the above-mentioned CNN training data, outputting the trained CNN weights `./cnn_weights_ceps_g711_best_example.h5`:
```bash
python CepsDomCNN_Train.py
```
- Note that your own dataset needs to replace the example speech files.

### Codecs and processing functions

- To obtain G.711-coded speech samples, some processing functions and the ITU-T G.711 codec are needed.
- Download the processing functions from [ITU-T G.191](https://www.itu.int/rec/T-REC-G.191-201003-I) and compile the relevant files to obtain the programs: `filter.exe`, `sv56demo.exe`, and `g711demo.exe`.
- Put the compiled programs in the root directory.
- Run the Matlab script to obtain G.711-coded speech, with a raw speech sample `./dataset/exapmle_s.raw` and the above-mentioned programs, outputting G.711-coded speech `./dataset/exapmle_s_g711_coded.raw`:
```bash
matlab CodedSpeech_Obtain.m.
```

## Citation

If you use the scripts in your research, please cite

```
@article{cnn2codedspeech,
  author =  {Z. Zhao and H. Liu and T. Fingscheidt},
  title =   {{Convolutional Neural Networks to Enhance Coded Speech}},
  howpublished = {\url{https://github.com/ZiyueZhao/coded_speech_enhance}}
}
```

## Acknowledgements
- The CNN topology used here is a deep encoder-decoder network, which is motivated from [Image Restoration Using Very Deep Convolutional Encoder-Decoder Networks with Symmetric Skip Connections](https://arxiv.org/abs/1603.09056).
- The authors would like to thank Samy Elshamy for the advice concerning the construction of the project in Github.