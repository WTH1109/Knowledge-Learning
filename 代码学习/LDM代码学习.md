# LDM代码学习

## 创建模型加载模型

将模型参数保存在config.yaml文件中，eg

```yaml
model:
  base_learning_rate: 2.0e-06
  target: ldm.models.diffusion.ddpm.LatentDiffusion
  params:
    linear_start: 0.0015
    linear_end: 0.0195
    ...
    unet_config:
      target: ldm.modules.diffusionmodules.openaimodel.UNetModel
      params:
        image_size: 64
        in_channels: 3
        ...
    first_stage_config:
      target: ldm.models.autoencoder.VQModelInterface
      params:
        embed_dim: 3
        n_embed: 8192
          ...
        lossconfig:
          target: torch.nn.Identity
    cond_stage_config:
      target: ldm.modules.encoders.modules.BERTEmbedder
      params:
        n_embed: 640
        n_layer: 32
data:
  target: main.DataModuleFromConfig
  params:
    batch_size: 28
    num_workers: 10
    wrap: false
    train:
      target: ldm.data.previews.pytorch_dataset.PreviewsTrain
      params:
        size: 256
    validation:
      target: ldm.data.previews.pytorch_dataset.PreviewsValidation
      params:
        size: 256

```

用以下的代码解析模型的参数

```python
def instantiate_from_config(config):
    if not "target" in config:
        if config == '__is_first_stage__':
            return None
        elif config == "__is_unconditional__":
            return None
        raise KeyError("Expected key `target` to instantiate.")
    return get_obj_from_str(config["target"])(**config.get("params", dict()))


def get_obj_from_str(string, reload=False):
    module, cls = string.rsplit(".", 1)
    if reload:
        module_imp = importlib.import_module(module)
        importlib.reload(module_imp)
    return getattr(importlib.import_module(module, package=None), cls)
```

``instantiate_from_config``解析config中的target属性，获得``ldm.models.diffusion.ddpm.LatentDiffusion``, ``importlib.import_module(module, package=None)``动态导入指定模块``ldm.models.diffusion.ddpm``,``getattr``从该文件中获取指定的方法``LatentDiffusion``，用``**config.get("params", dict())``的方式解析``params``里的参数，并传入用``get_obj_from_str``得到的``LatentDiffusion``方法当中，从而完成模型参数的加载