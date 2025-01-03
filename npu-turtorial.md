环境信息：

1.  NPU 基础镜像
2. 依赖库
	1. Basic
		1. `pip install -r verl/requirements`
		2. `pip install pandas pyarrow`
	2. vLLM on NPU
		1. https://github.com/vllm-project/vllm/pull/8054
		2. `git clone -b npu_support https://github.com/wangshuai09/vllm.git`
		3. `cd vllm & VLLM_TARGET_DEVICE=npu pip install -e .`
		4. 侵入式修改 `vllm/engine/llm_engine.py` 需要注掉 `if max_input_id > tokenizer.max_token_id` 相关判断
	3. Megatron
		1. `git clone https://github.com/NVIDIA/Megatron-LM.git`
		2. `git checkout core_r0.6.0`
		3. `git apply verl/patches/megatron_v0.6_npu.patch`
		4. `pip install -e .`
	4. Mindspeed
		1. `git clone https://gitee.com/ascend/MindSpeed.git`
		2. `git checkout 969686f`
		3. `pip install -e .`
	5. Ascend-Apex
		1. `git clone https://gitee.com/ascend/apex.git ascend-apex`
		2. `cd ascend-apex && bash scripts/build.sh --python=3.10`
		3. install
	6. veRL
		1. `git clone https://github.com/chendong-1998/verl.git`
		2. `cd verl && git checkout support-ascend-npu`

然后可以配置模型、数据集路径后，执行 `bash example/ppo_trainer/run_deepseek_megatron.sh` 即可在 NPU 上运行。