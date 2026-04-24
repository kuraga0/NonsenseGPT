
# nonsenseGPT

*Forked from [nanoGPT](https://github.com/karpathy/nanoGPT)*

## setup

We are using [uv](https://docs.astral.sh/uv/getting-started/installation/) for dependency management.

``` sh
uv venv --python 3.12.3
```

``` sh
source .venv/bin/activate
# or
source .venv/bin/activate.fish
```

``` sh
uv pip install torch --default-index https://download.pytorch.org/whl/cpu 
uv pip install numpy transformers datasets tiktoken wandb tqdm
```

Dependencies:

- [pytorch](https://pytorch.org) <3
- [numpy](https://numpy.org/install/) <3
-  `transformers` for huggingface transformers <3 (to load GPT-2 checkpoints)
-  `datasets` for huggingface datasets <3 (if you want to download + preprocess OpenWebText)
-  `tiktoken` for OpenAI's fast BPE code <3
-  `wandb` for optional logging <3
-  `tqdm` for progress bars <3

## quick start

Thsi instructions are for CPU training only because i dont have dedicated GPU.

```sh
python data/nontrinsic_char/prepare.py
```

This creates a `train.bin` and `val.bin` in that data directory. Now it is time to train your GPT. The size of it very much depends on the computational resources of your system:

If you peek inside it, you'll see that we're training a GPT with a context size of up to 256 characters, 384 feature channels, and it is a 6-layer Transformer with 6 heads in each layer. On one A100 GPU this training run takes about 3 minutes and the best validation loss is 1.4697. Based on the configuration, the model checkpoints are being written into the `--out_dir` directory `out-shakespeare-char`. So once the training finishes we can sample from the best model by pointing the sampling script at this directory:

```sh
python train.py config/train_nontrinsic_char.py --device=cpu --compile=False --max_iters=20000 
```

```sh
python sample.py --out_dir=out-nontrinsic-char --device=cpu --num_samples=5 --max_new_tokens=600 --temperature=0.9 --start="Hello"
```

This generates a few samples, for example:

```
Hellops. You lose so that the dead bose are you do I am a bord a cinters destrops, very nice.
greates down see the open on is a sygraming sense 20 h day obsital of actional many beltter a time difficult? How do the Fromacked on Helrous all because to have a new að ð¥ ð¤¬ ð¥ ð°ð
He Linux message me, and while I am the sittly
Aluminium is stow, you stall understand me start. Opend
Metion, house what enter you will i should entries, there use sconver in the people the plance in cover my dominally of nonsenses is real the cantain makesense, nonsense screen what i'm plan, look it was a part
---------------
Hello, I have exaniate a know, what think users on this is!
We not not onsense safe. Everyone everything should to about my disteast depenored includes; It's profects fungerst of until the patfor, something instead and minit is on you for to be free hundrink, all the what you it said
epty !
that something designice (what has grobed someone is that we're to be to.
But with you can be about you're read it wordo what and in making with the being of duble them, and dienter is for the univeRsion
Love informations, loady for a busing on the divice to profict of the two played in the namey good (so-perfor
---------------
Hello Head Here Ledays Ditconscords, Then future the person is a That?
Books Waitific it was about with a sdaw time.
A wait of the step What not awdanted day
what tleep the break an face of out context that is got OD.
And it acceptivide traesming the reound to like that a starts are one the buile internant of anything in there integrally eaters sol
My grink p frain, but it much someone took read mark post to be the pooking in they ito is got sense and nonsense.
Nonsense ears protes in just the project, it's a currest contRibutive and used
he month
Give you take post of the makes is a people windows
---------------
```
