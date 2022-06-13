#  HW Readme

End-to-end (E2E) automatic speech recognition (ASR) systems often have difficulty recognizing uncommon words, that appear infrequently in the training data. One promising method, to improve the recognition accuracy on such rare words, is to latch onto personalized/contextual information at inference.In this work, we present a novel approach that we can use Keyword prefix tree Search in BeamSearch.
 
This repository contains:

1. Necessary Files.
2. Documents for Install and Use.

## Configure the ESPnet environment

- Download Espnet
```sh
$ git clone https://github.com/espnet/espnet.git
```
- Install Conda & Create the Conda environment
```sh
$ conda create -n espnet python=3.8
$ conda activate espnet
$ conda install pytorch==1.6.0 cudatoolkit=10.1 torchaudio=0.6.0 -c pytorch

# GPU 3090 is available ,Perform the following configuration
$ conda create -n espnet python=3.8
$ conda activate espnet
$ pip install -r requirements.txt
$ conda install pytorch torchvision torchaudio=0.8.0 cudatoolkit=11.1 -c pytorch -c conda-forge
```

- Config Espnet

You can follow this Document(https://espnet.github.io/espnet/installation.html)

## Usage

Firstly, You should prepare a file of oov-list which like below:

```sh
ALEXANDER LAURIE JOHNSTON
ALEXANDRA LANE
ALEXANDRA ROAD
ALIMAN HASSAN
ALJUNIED AVENUE
ALJUNIED CRESCENT
ALJUNIED HUSSEIN
```

Then You should convert this file to bpe piece:

```sh
_A LE X _JO SE Y
_A LE X AND ER _LA UR IE _JOHN S TON
_A LE X AND RA _LANE
_A LE X AND RA _ROAD
_A LI MAN _HAS S AN
_AL JU NI ED _AVENUE
_AL JU NI ED _C RE S C ENT
_AL JU NI ED _H US SE IN
```

You need put hotword_fst.py in
```sh
# $espnet_folder/espnet/nets/scorers/
```

And put asr_inference_fst.py in 
```sh
# $espnet_folder/espnet2/bin/
```

If you want use keyword prefix tree search in BeamSearch, You should modify espnet/egs2/TEMPLATE/asr1/asr.sh in column 1244~1245 as following and should specify the hwfst_weight:

```sh
# 2. Submit decoding jobs
log "Decoding started... log: '${_logdir}/asr_inference.*.log'"
# shellcheck disable=SC2086
echo "##cmd##" ${_cmd}
${_cmd}  JOB=1:"${_nj}" "${_logdir}"/asr_inference.JOB.log \
         ${python} -m espnet2.bin.asr_inference_fst \
             --batch_size ${batch_size} \
             --data_path_and_name_and_type "${_data}/${_scp},speech,${_type}" \
             --key_file "${_logdir}"/keys.JOB.scp \
             --asr_train_config "${asr_exp}"/config.yaml \
             --asr_model_file "${asr_exp}"/"${inference_asr_model}" \
             --output_dir "${_logdir}"/output.JOB \
             --hwfst_weight $hwfst_weight \
             --hw_list_path "data/hw_piece" \
             ${_opts} ${inference_args}
```


## Example

To see how the specification has been applied, see the [example-readmes](example-readmes/).

## Related Efforts



## Maintainers



## Contributing



### Contributors




## License

[MIT](LICENSE) © Richard Littauer
