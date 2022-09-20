# Dreambooth on Stable Diffusion. CPU version
Slight changes were made to make the script work on windows using CPU.

Usage and requirements are the same as the GPU version at https://github.com/XavierXiao/Dreambooth-Stable-Diffusion

Generate regularization images:
```
python scripts/stable_txt2img.py --ddim_eta 0.0 --n_samples 8 --n_iter 1 --scale 10.0 --ddim_steps 50  --ckpt /path/to/original/stable-diffusion/sd-v1-4-full-ema.ckpt --prompt "a photo of a <class>" 

#<class> = dog or whatever your object is
```
Took me 2 hours on CPU, i suggest using a GPU-script to generate them in a minute

Training:
```
python main.py --base configs/stable-diffusion/v1-finetune_unfrozen.yaml -t --actual_resume ./models/ldm/stable-diffusion-v1/model.ckpt -n <job name>  --data_root ./path/to/training/data --reg_data_root ./path/to/regularization --class_word <class> --logdir ./logs/
```

When you get the model (about 12gb) you can prune it using the waifu-diffusion script to get a 2gb model
```
python ./scripts/prune.py /path/to/model.ckpt
```

The model can be used on any other stable-diffusion repo with the prompt 
```
a photo of a sks <class>
```


Performance are obviously way slower:
about 6-7 hours for 500 steps on a ryzen 3900x at 3.6ghz and 48GB of RAM (30-35GB used).

500 steps are usually enough.

if you haven't enough RAM, you can probably allocate virtual RAM on a SSD but with a huge speed decrease, i haven't tried yet.
