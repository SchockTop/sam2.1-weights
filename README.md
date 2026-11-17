# SAM 2.1 weights

Unmodified copies of Meta's model weights, stored directly in this git repository. Upstream: https://github.com/facebookresearch/sam2

**License:** these files are distributed under the Apache License 2.0, see [LICENSE](LICENSE).
This mirror does not relicense anything.

## Get the weights

Everything is in the repo itself (no releases, no Git LFS). GitHub allows at most 100 MB per file,
so each large file is stored as `<name>.part000`, `<name>.part001`, ... next to its config files.

```
git clone --depth 1 https://github.com/SchockTop/sam2.1-weights.git
cd sam2.1-weights
python join.py
```

`join.py` works offline (standard library only): it joins the parts into the model files below and checks
the SHA256. `python join.py --only <folder>` joins one variant, `--delete-parts` frees the space afterwards.

Without Python, join by hand and compare the SHA256 with the table:

```
:: Windows (cmd)
copy /b "sam2.1-hiera-large-hf\model.safetensors.part*" "sam2.1-hiera-large-hf\model.safetensors"
certutil -hashfile "sam2.1-hiera-large-hf\model.safetensors" SHA256

# Linux / macOS
cat sam2.1-hiera-large-hf/model.safetensors.part* > sam2.1-hiera-large-hf/model.safetensors && sha256sum sam2.1-hiera-large-hf/model.safetensors
```

## Contents

- `sam2.1-hiera-large-hf/`: SAM 2.1 Hiera Large in Hugging Face format (`model.safetensors` + configs,
  `sam2.1_hiera_l.yaml` and the upstream model card), from `facebook/sam2.1-hiera-large`,
  revision `665f8e2ad61cf5f53d65644ff27c8ee525124610`
- `sam2.1-hiera-large-onnx/`: the same model as ONNX (encoder + decoder + `config.yaml`) for
  [AnyLabeling](https://github.com/vietanhdev/anylabeling) and `onnxruntime`. Unmodified content of
  `sam2.1_hiera_large_20260221.zip` from https://huggingface.co/vietanhdev/segment-anything-2.1-onnx-models
  (export by Viet-Anh Nguyen, Apache 2.0)

| File | Size | Parts | SHA256 |
|---|---|---|---|
| `sam2.1-hiera-large-hf/model.safetensors` | 0.84 GB | 10 | `dc407dce21301fd94abb395c5099b4f2c455fdc8a8f261ac3d0ea6d4cd197230` |
| `sam2.1-hiera-large-onnx/sam2.1_hiera_large.encoder.onnx` | 0.79 GB | 9 | `f18538735868c076b715d5abbdf1a9d5fb4b0939c00be9db42d6b3808cdd3251` |
| `sam2.1-hiera-large-onnx/sam2.1_hiera_large.decoder.onnx` | 16 MB | stored whole | `014ac31e9037238ee027d6399d932dafe80c15ae711aee564976b94383b4ffa1` |

## Use the ONNX model in AnyLabeling

```
python join.py --only sam2.1-hiera-large-onnx
```

AnyLabeling normally downloads this model itself. Offline, copy the three files of `sam2.1-hiera-large-onnx/`
(`config.yaml`, encoder, decoder; not the parts) into
`<home>/anylabeling_data/models/sam2.1_hiera_large_20260221/`, or load `config.yaml` in AnyLabeling via
"Load Custom Model". Inputs: encoder `image` 1x3x1024x1024; decoder takes the embeddings plus point prompts.
