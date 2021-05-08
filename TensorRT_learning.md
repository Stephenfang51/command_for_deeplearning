<h1 align=center>模型加速</h1>
<h5 align = right> update 2020.4.3</h5>

鉴于没有一个好的教程， 网上简体中文教程多半机械式翻译， 跟垃圾差不多， 固自己记录一份并与官方文档及各博客进行整合, 在做模型转换之前， 请先确认你的onnx model 是那个版本torch进行转换的， 如果是1.3， 则TensorRT6并不支持， 

> ### 目录
>
> ### [TensorRT](1)
>
> 1. TensorRT 安装步骤
> 2. TensorRT 基础使用（中文）
> 3. TensorRT core library （官方内容）
> 4. Building An Engine In C++
> 5. Adding A Custom Layer To Your Network In TensorRT
> 6. Performing Inference In INT8 Using Custom Calibration
> 7. TensorRT API
>    1. 
>
> ### torch.onnx
>
> 1. 基础使用
>
> ### Libtorch
>
> 
>
> ### Onnxruntime
>
> 
>
> ### Onnx-TensorRT
>
> 1. Onnx-TensorRT 安装步骤
> 2. onnx转tensorrt 自定义op
>
> 
>
> ### Torch2Trt
>
> //TODO 





<h1 id=1> TensorRT</h1>

> ### TensorRT 安装步骤

依照CUDA版本下载相对应的CUDNN

1. 下载对应cuda版本的cudnn

   ```
   tar -xvzf cudnn-10.0-linux-x64-v7.6.5.32.tgz -C 目的path
   #解压缩出来会是一个cuda资料夹
   cd 到cuda下
   
   sudo cp include/cudnn.h /usr/local/cuda/include/
   sudo cp lib64/lib* /usr/local/cuda/lib64/
   
   
   cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2 #测试
   ```

   

2. 下载适合环境的TensorRT版本以及安装

   - 到官方下载适合环境的TensorRT  https://developer.nvidia.com/tensorrt

   - 下载好之后 解压缩

     `tar -xzvf TensorRT-7.0.0.11.Ubuntu-16.04.x86_64-gnu.cuda-10.0.cudnn7.6.tar.gz -C /指定你要的路径`

   - 添加环境变量 `vim ~/.bashrc`

     例如 `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/你安装的路径/TensorRT-7.0.0.11/lib`

     之后记得`source ~/.bashrc`一下

   - `cd TensorRT-7.x.x.x/python `

   - 依照python版本选择相应的安装， ex `pip install tensorrt-6.0.1.5-cp36-none-linux_x86_64.whl`



> ### 中途遇见的问题

**问题1**

pip install tensorrt-6.0.1.5-cp36-none-linux_x86_64.whl 的时候出问题

报错如下

ERROR: tensorrt-6.0.1.5-cp36-none-linux_x86_64.whl is not a supported wheel on this platform

这通常只会有两个原因， 但大部分是第一个原因， 也就是python环境选错了， 例如python3.5 则安装3.6的

注意如果使用conda可能会遇到该问题， 在conda下安装请使用pip install, 不要用pip3



> ### TensorRT基础使用（中文）
>
> 各版本官方文档https://s0docs0nvidia0com.icopy.site/deeplearning/sdk/tensorrt-release-notes/index.html
>
> 内容大部分参考截取自 https://murphypei.github.io/blog/2019/09/trt-useage



TRT 将模型结构和参数以及相应 kernel 计算方法都编译成一个二进制 engine，因此在部署之后大大加快了推理速度。为了能够使用 TRT 进行推理，需要创建一个 eninge。TRT 中 engine 的创建有两种方式

- 通过网络模型结构和参数文件编译得到，比较慢。
- 读取一个已有的 engine（gie 文件），因为跳过了模型解析等过程，速度更快。



现在假设我们是第一次用 TRT，所以就只能选择第一种方式来创建一个 engine。为了创建一个 engine，我们需要有模型结构和模型参数两个文件，同时需要能够解析这两个文件的方法。在 TRT 中，编译 engine 是通过 `IBuilder` 对象进行的，因此我们首先需要新键一个 `IBuilder` 对象：

```
nvinfer1::IBuilder *builder = createInferBuilder(gLogger);
```

> `gLogger` 是 TRT 中的日志接口 `ILogger` ，继承这个接口并创建自己的 logger 对象传入即可。

为了编译一个 engine，`builder` 首先需要创建一个 `INetworkDefinition` 作为模型的容器：

```
nvinfer1::INetworkDefinition *network = builder->createNetwork();
```

注意，**此时 network 是空的**，我们需要填充模型结构和参数，也就是解析我们自己的模型结构和参数文件，获取数据放到其中。

TRT 官方给了三种主流框架模型格式的解析器（parser），分别是：

- ONNX：`IOnnxParser parser = nvonnxparser::createParser(*network, gLogger);`
- Caffe：`ICaffeParser parser = nvcaffeparser1::createCaffeParser();`
- UFF：`IUffParser parser = nvuffparser::createUffParser();`

其中 UFF 是用于 TensorFlow 的格式。调用这三种解析器就可以解析相应的文件。以 `ICaffeParser` 为例，调用其 `parse` 方法来填充 `network`。

```C++
virtual const IBlobNameToTensor* nvcaffeparser1::ICaffeParser::parse(
    const char* deploy, 
    const char * model, 
	nvinfer1::INetworkDefinition &network, 
	nvinfer1::DataType weightType)

//Parameters
//deploy	    The plain text, prototxt file used to define the network configuration.
//model	        The binaryproto Caffe model that contains the weights associated with the network.
//network	    Network in which the CaffeParser will fill the layers.
//weightType    The type to which the weights will transformed.
```

这样就能得到一个填充好的 `network` ，就可以编译 engine 了，似乎一切都很美妙呢…

然而实际 TRT 并不完善，比如 TensorFlow 的很多操作并不支持，因此你传入的文件往往是根本就解析不了（深度学习框架最常见的困境之一）。因此我们需要自己去做填充 `network` 这件事，这就需要调用 TRT 中低级别的接口来创建模型结构，类似于你在 Caffe 或者 TensorFlow 中做的那样。



TRT 提供了较为丰富的接口让你可以直接通过这些接口创建自己的网络，比如添加一个卷积层：

```C++
virtual IConvolutionLayer* nvinfer1::INetworkDefinition::addConvolution(ITensor &input, 
                                                                        int nbOutputMaps,
                                                                        DimsHW kernelSize,
                                                                        Weights kernelWeights,
                                                                        Weights biasWeights)		

// Parameters
// input	The input tensor to the convolution.
// nbOutputMaps	The number of output feature maps for the convolution.
// kernelSize	The HW-dimensions of the convolution kernel.
// kernelWeights	The kernel weights for the convolution.
// biasWeights	The optional bias weights for the convolution.
```

这里的参数基本上就是和其他深度学习框架类似的意思，没有什么好讲的。就是把数据封装成 TRT 中的数据结构即可。可能和平时构建训练网络不同的地方就是需要填充好模型的参数，<mark>因为 TRT 是推理框架，参数是已知确定的</mark>。这个过程一般是读取已经训练好的模型，构造 TRT 的数据结构类型放到其中，也就是需要你自己去解析模型参数文件。

之所以说 TRT 的网络构造接口是**较为丰富**，是因为即使使用这些低级接口这样，很多操作还是没办法完成，也就是没有相应的 `add*` 方法，更何况现实业务可能还会涉及很多自定义的功能层，因此 TRT 又有了 plugin 接口，允许你自己定义一个 `add*` 的操作。其流程就是继承 `nvinfer1::IPluginV2` 接口，利用 cuda 编写一个自定义层的功能，然后继承 `nvinfer1::IPluginCreator` 编写其创建类，需要重写其虚方法 `createPlugin`。最后调用 `REGISTER_TENSORRT_PLUGIN` 宏来注册这个 plugin 就可以用了。plugin 接口的成员函数介绍。



```C++
// 获得该自定义层的输出个数，比如 leaky relu 层的输出个数为1
virtual int getNbOutputs() const = 0;

// 得到输出 Tensor 的维数
virtual Dims getOutputDimensions(int index, const Dims* inputs, int nbInputDims) = 0;

// 配置该层的参数。该函数在 initialize() 函数之前被构造器调用。它为该层提供了一个机会，可以根据其权重、尺寸和最大批量大小来做出算法选择。
virtual void configure(const Dims* inputDims, int nbInputs, const Dims* outputDims, int nbOutputs, int maxBatchSize) = 0;

// 对该层进行初始化，在 engine 创建时被调用。
virtual int initialize() = 0;

// 该函数在 engine 被摧毁时被调用
virtual void terminate() = 0;

// 获得该层所需的临时显存大小。
virtual size_t getWorkspaceSize(int maxBatchSize) const = 0;

// 执行该层
virtual int enqueue(int batchSize, const void*const * inputs, void** outputs, void* workspace, cudaStream_t stream) = 0;

// 获得该层进行 serialization 操作所需要的内存大小
virtual size_t getSerializationSize() = 0;

// 序列化该层，根据序列化大小 getSerializationSize()，将该类的参数和额外内存空间全都写入到序列化buffer中。
virtual void serialize(void* buffer) = 0;
```

我们需要根据自己层的功能，重写这里全部或者部分函数的实现，这里有很多细节，没办法一一展开，需要自定义的时候还是需要看官方 API。

构建好了网络模型，就可以执行 engine 的编译了，还需要对 engine 进行一些设置。比如计算精度，支持的 batch size 等等，因为这些设置不同，编译出来的 engine 也不同。

TRT 支持 FP16 计算，也是官方推荐的计算精度，其设置也比简单，直接调用：



```
builder->setFp16Mode(true);
```

另外在设置精度的时候，还有一个设置 strict 策略的接口：

```
builder->setStrictTypeConstraints(true);
```

这个接口就是是否严格按照设置的精度进行类型转换，如果不设置 strict 策略，则 TRT 在某些计算中可能会选择更高精度（不影响性能）的计算类型。

除了精度，还需要设置好运行的 batch size 和 workspace size：

```
builder->setMaxBatchSize(batch_size);
builder->setMaxWorkspaceSize(workspace_size);
```

这里的 batch size 是运行时最大能够支持的 batch size，运行时可以选择比这个值小的 batch size，workspace 也是相对于这个最大 batch size 设置的。

设置好上述参数，就可以编译 engine 了。

```
nvinfer1::ICudaEngine *engine = builder->buildCudaEngine(*network);
```

编译需要花较长时间，耐心等待。



### Engine 序列化和反序列化

编译 engine 需要较长时间，在模型和计算精度、batch size 等均保持不变的情况下，我们可以选择保存 engine 到本地，供下次运行使用，也就是 engine 序列化。TRT 提供了很方便的序列化方法：



```
nvinfer1::IHostMemory *modelStream = engine->serialize();
```

通过这个调用，得到的是一个二进制流，将这个流写入到一个文件中即可保存下来。

如果需要再次部署，可以直接反序列化保存好的文件，略过编译环节。

```
IRuntime* runtime = createInferRuntime(gLogger);
ICudaEngine* engine = runtime->deserializeCudaEngine(modelData, modelSize, nullptr);
```

### 使用 engine 进行预测

有了 engine 之后就可以使用它进行 inference 了。

首先创建一个 inference 的 context。这个 context 类似命名空间，用于保存一个 inference 任务的变量。

```
IExecutionContext *context = engine->createExecutionContext();
```

**一个 engine 可以有多个 context**，也就是说一个 engine 可以同时进行多个预测任务。

然后就是绑定输入和输出的 index。这一步的原因在于 TRT 在 build engine 的过程中，将输入和输出映射为索引编号序列，因此我们只能通过索引编号来获取输入和输出层的信息。虽然 TRT 提供了通过名字获取索引编号的接口，但是本地保存可以方便后续操作。

我们可以先获取索引编号的数量：

```
int index_number = engine->getNbBindings();
```

我们可以判断这个编号数量是不是和我们网络的输入输出之和相同，比如你有一个输入和一个输出，那么编号的数量就是2。如果不是，则说明这个 engine 是有问题的；如果没问题，我们就可以通过名字获取输入输出对应的序号：

```
int input_index = engine->getBindingIndex(input_layer_name);
int output_index = engine->getBindingIndex(output_layer_name);
```

对于常见的一个输入和输出的网络，输入的索引编号就是 0，输出的索引编号就是 1，所以这一步也不是必须的。

接下来就需要为输入和输出层分配显存空间了。为了分配显存空间，我们需要知道输入输出的维度信息和存放的数据类型，TRT 中维度信息和数据类型的表示如下：

```c++
class Dims
{
public:
    static const int MAX_DIMS = 8; //!< The maximum number of dimensions supported for a tensor.
    int nbDims;                    //!< The number of dimensions.
    int d[MAX_DIMS];               //!< The extent of each dimension.
    DimensionType type[MAX_DIMS];  //!< The type of each dimension.
};

enum class DataType : int
{
    kFLOAT = 0, //!< FP32 format.
    kHALF = 1,  //!< FP16 format.
    kINT8 = 2,  //!< quantized INT8 format.
    kINT32 = 3  //!< INT32 format.
};
```

我们通过索引编号获取输入和输出的数据维度（dims）和数据类型（dtype），然后为每个输出层开辟显存空间，存放输出结果：

```
for (int i = 0; i < index_number; ++i)
{
	nvinfer1::Dims dims = engine->getBindingDimensions(i);
	nvinfer1::DataType dtype = engine->getBindingDataType(i);
    // 获取数据长度
    auto buff_len = std::accumulate(dims.d, dims.d + dims.nbDims, 1, std::multiplies<int64_t>());
    // ...
    // 获取数据类型大小
    dtype_size = getTypeSize(dtype);	// 自定义函数
}

// 为 output 分配显存空间
for (auto &output_i : outputs)
{
    cudaMalloc(buffer_len_i * dtype_size_i * batch_size);
}
```

> 本文给出的是伪代码，仅表示逻辑，因此会涉及一些简单的自定义函数。

至此，我们已经做好了准备工作，现在就可以把数据塞进模型进行推理了。

#### 前向预测

TRT 的前向预测执行是异步的，context 通过一个 enqueue 调用来提交任务：

```
cudaStream_t stream;
cudaStreamCreate(&stream);
context->enqueue(batch_size, buffers, stream, nullptr);
cudaStreamSynchronize(stream);
```

enqueue 是 TRT 的实际执行任务的函数，我们在写 plugin 的时候也需要实现这个函数接口。其中：

- `batch_size`：engine 在 build 过程中传入的 `max_batch_size`。

- `buffers`：是一个指针数组，其下标对应的就是输入输出层的索引编号，存放的就是输入的数据指针以及输出的数据存放地址（也就是开辟的显存地址）。

- `stream`：stream 是 cuda 一系列顺序操作的概念。对于我们的模型来说就是将所有的模型操作按照（网络结构）指定的顺序在指定的设备上执行。

  > cuda stream 是指一堆异步的 cuda 操作，他们按照 host 代码调用的顺序执行在 device 上。stream 维护了这些操作的顺序，并在所有预处理完成后允许这些操作进入工作队列，同时也可以对这些操作进行一些查询操作。这些操作包括 host 到 device 的数据传输，launch kernel 以及其他的 host 发起由 device 执行的动作。这些操作的执行总是异步的，cuda runtime 会决定这些操作合适的执行时机。我们则可以使用相应的cuda api 来保证所取得结果是在所有操作完成后获得的。**同一个 stream 里的操作有严格的执行顺序**，不同的 stream 则没有此限制。

这里需要注意，输入数据和输出数据在 buffers 数组中都是在 GPU 上的，可以通过 `cudaMemcpy` 拷贝 CPU 上的输入数据到 GPU 中（需要提前开辟一块显存来存放）。同理，输出数据也需要从 GPU 中拷贝到 CPU 中。

前两句创建了一个 cuda stream，最后一句则是等待这个异步 stream 执行完毕，然后从显存中将数据拷贝出来即可。

至此，我们就完成了 TRT 一个基本的预测流程。

### 总结

本文仅仅是针对 TRT 的预测流程和一些常见调用进行了说明，并不涉及具体网络和具体实现，也没有太多编码的细节。不同网络不同操作需要一些扩展 plugin 的编写，而对于编码，包括内存和显存的开辟管理，TRT 的析构清理工作等等都不在本文叙述范围之内。

#### 参考资料：

- https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#c_topics
- https://docs.nvidia.com/deeplearning/sdk/tensorrt-api/c_api/index.html
- https://www.cnblogs.com/1024incn/p/5891051.html
- **本文作者：** murphypei
- **本文链接：** https://murphypei.github.io/blog/2019/09/trt-useage.html
- **版权声明：** 本博客所有文章除特别声明外，均采用 [BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明出处！





------

以下为官方内容



> ### TensorRT core library

主要分为下列三个部分

- Network Definition

  The Network Definition interface provides methods for the application to specify the definition of a network. Input and output tensors can be specified, layers can be added, and there is an interface for configuring each supported layer type. As well as layer types, such as convolutional and recurrent layers, and a Plugin layer type allows the application to implement functionality not natively supported by TensorRT. For more information about the Network Definition, see [Network Definition API](http://docs.nvidia.com/deeplearning/sdk/tensorrt-api/c_api/classnvinfer1_1_1_i_network_definition.html).

- Builder

  The Builder interface allows the creation of an optimized engine from a network definition. It allows the application to specify the maximum batch and workspace size, the minimum acceptable level of precision, timing iteration counts for autotuning, and an interface for quantizing networks to run in 8-bit precision. For more information about the Builder, see [Builder API](http://docs.nvidia.com/deeplearning/sdk/tensorrt-api/c_api/classnvinfer1_1_1_i_builder.html).

- Engine

  The Engine interface allows the application to execute inference. It supports synchronous and asynchronous execution, profiling, and enumeration and querying of the bindings for the engine inputs and outputs. A single-engine can have multiple execution contexts, allowing a single set of trained parameters to be used for the simultaneous execution of multiple batches. For more information about the Engine, see [Execution API](https://docs.nvidia.com/deeplearning/sdk/tensorrt-api/c_api/classnvinfer1_1_1_i_cuda_engine.html).

> ### C++ VS python

The C++ API should be used in any performance-critical scenarios, as well as in situations where safety is important, for example, in automotive.

The main benefit of the Python API is that data preprocessing and postprocessing are easy to use because you’re able to use a variety of libraries like NumPy and SciPy. For more information about the Python API, see [Using The Python API](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#python_topics).





> ### Building An Engine In C++

The next step is to invoke the TensorRT builder to create an optimized runtime. One of the functions of the builder is to search through its catalog of CUDA kernels for the fastest implementation available, and thus it is **necessary to use the same GPU for building like that on which the optimized engine will run.**

编译engine的以及使用的 必须在同一个GPU



Two particularly important properties are the maximum batch size and the maximum workspace size.



#### 1. Build the engine using the builder object:

```
builder->setMaxBatchSize(maxBatchSize);
IBuilderConfig * config = builder->createBuilderConfig();
config->setMaxWorkspaceSize(1 << 20);
ICudaEngine* engine = builder->buildEngineWithConfig(*network, *config);
```

When the engine is built, TensorRT makes copies of the weights.







#### 2. Serializing A Model In C++ 序列化engine来存储

It is not absolutely necessary to serialize and deserialize a model before using it for inference – if desirable, the engine object can be used for inference directly.

To [serialize](https://en.wikipedia.org/wiki/Serialization), you are transforming the engine into a format to store and use at a later time for inference. To use for inference, you would simply deserialize the engine. Serializing and deserializing are optional. Since creating an engine from the Network Definition can be time consuming, you could avoid rebuilding the engine every time the application reruns by serializing it once and deserializing it while running inference. Therefore, after the engine is built, users typically want to serialize it for later use.

**Note:** Serialized engines are not portable across platforms or TensorRT versions. Engines are specific to the exact GPU model they were built on (in addition to the platforms and the TensorRT version).



1. Run the builder as a prior offline step and then serialize:

   ```
   IHostMemory *serializedModel = engine->serialize();
   // store model to disk
   // <…>
   serializedModel->destroy();
   ```

2. Create a runtime object to deserialize:

   ```
   IRuntime* runtime = createInferRuntime(gLogger);
   ICudaEngine* engine = runtime->deserializeCudaEngine(modelData, modelSize, nullptr);
   ```

   The final argument is a plugin layer factory for applications using custom layers. For more information, see [Extending TensorRT With Custom Layers](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#extending).

   

   **如果模型有包含plugin, 例如DCNv2**

   记得如果输入的onnx， 是包含plugin的，必须先调用 createPluginFactory， 例如

   ```c++
   //需要先创建PluginFactory 
   mPlugins = nvonnxparser::createPluginFactory(gLogger);
   
   //接着desrializeCudaEngine 第三个参数需要加上
   mEngine= mRunTime->deserializeCudaEngine(modelStream->data(), modelStream->size(), mPlugins);
   ```

   否则在deserialized engine的过程会报以下错误

   ```shell
   ERROR: Serialized engine contains plugin, but no plugin factory was provided. To deserialize an engine without a factory, please use IPluginV2 instead.
   ```

   



#### 3. Performing Inference In C++

The following steps illustrate how to perform inference in C++ now that you have an engine.



1. Create some space to store intermediate activation values. Since the engine holds the network definition and trained parameters, additional space is necessary. These are held in an execution context:

   ```
   IExecutionContext *context = engine->createExecutionContext();
   ```

   An engine can have multiple execution contexts, allowing one set of weights to be used for multiple overlapping inference tasks. For example, you can process images in parallel CUDA streams using one engine and one context per stream. Each context will be created on the same GPU as the engine.

2. Use the input and output blob names to get the corresponding input and output index:

   ```
   int inputIndex = engine->getBindingIndex(INPUT_BLOB_NAME);
   int outputIndex = engine->getBindingIndex(OUTPUT_BLOB_NAME);
   ```





------

> ### Adding A Custom Layer To Your Network In TensorRT
>
> 参考tensorrt 5.1 https://github.com/NVIDIA/TensorRT/tree/release/5.1/samples/opensource/samplePlugin#adding-a-custom-layer-to-your-network-in-tensorrt

该章节主要示范如何新增FC layer plugin到自己的网络中



流程

- [Defines the network](https://github.com/NVIDIA/TensorRT/tree/release/5.1/samples/opensource/samplePlugin#defining-the-network)
- [Enables custom layers](https://github.com/NVIDIA/TensorRT/tree/release/5.1/samples/opensource/samplePlugin#enabling-custom-layers-in-nvcaffeparser)
- [Builds the engine](https://github.com/NVIDIA/TensorRT/tree/release/5.1/samples/opensource/samplePlugin#building-the-engine)
- [Serialize and deserialize](https://github.com/NVIDIA/TensorRT/tree/release/5.1/samples/opensource/samplePlugin#serializing-and-deserializing4)
- [Initializes the plugin and executes the custom layer](https://github.com/NVIDIA/TensorRT/tree/release/5.1/samples/opensource/samplePlugin#resource-management-and-execution)

### Defining the network

The `FCPlugin` redefines the InnerProduct layer, which has a single output. Accordingly, `getNbOutputs` returns `1` and `getOutputDimensions` includes validation checks and returns the dimensions of the output:

```c++
Dims getOutputDimensions(int index, const Dims* inputDims,
						 int nbInputDims) override  
{  
	assert(index == 0 && nbInputDims == 1 &&
		   inputDims[0].nbDims == 3);  
	assert(mNbInputChannels == inputDims[0].d[0] *
							   inputDims[0].d[1] *
							   inputDims[0].d[2]);  
	return DimsCHW(mNbOutputChannels, 1, 1);  
}
```



//TODO 未完成

------



> ### Performing Inference In INT8 Using Custom Calibration

可参阅https://arleyzhang.github.io/articles/923e2c40/



一般深度学习框架训练都以float32为主，为什么要使用低精度推理int8?

- 降低计算量
- 减低推断时间



TensorRT只支持6.1以上的计算力， 

|      | 动态范围                                   | 最小正数              |
| ---- | ------------------------------------------ | --------------------- |
| FP32 | $-3.4 \times 10^{38} + 3.4 \times 10^{38}$ | $1.4 \times 10^{-45}$ |
| FP16 | $-65504 + 65504$                           | $5.96 \times 10^{-8}$ |
| INT8 | $-128 + 127$                               | 1                     |

原来一个tensor用32bit来表示， 现在打算用8bit来表示

而其实就是一种信息再编码的过程



![image-20191230164743898](/Users/stephenfang/Library/Application Support/typora-user-images/image-20191230164743898.png)



- 这种做法不是将 ±|max| 映射为 ±127，而是存在一个 阈值 |T| ，将 ±|T| 映射为±127，显然这里 |T|<|max|。
- 超出 阈值 ±|T| 外的直接映射为阈值 ±127。比如上图中的三个红色点，直接映射为-127。
- 称这种映射关系为饱和的（Saturate ），不对称的。
- 只要 阈值 选取得当，就能将分布散乱的较大的激活值舍弃掉，也就有可能使精度损失不至于降低太多。



对weights 的int8量化使用非饱和方式

对activation 使用饱和的方式



FP32的情况下最能表达tensor的最佳分布， 而现在要用int8开表达， 随着阈值T的取值会有多种的情况产生， 而就是要将最符合FP32分布的情况给找出来



我们用一个衡量指标来衡量不同的int8分布与原来的FP32分布之间的差异程度， 该衡量指标为相对熵 relative entropy, 也叫做KL离散

- 假设我们要给一个信息进行完美编码，那么最短平均编码长度就是信息熵。

- 如果编码方案不一定完美（由于对概率分布的估计不一定正确），这时的平均编码长度就是交叉熵。

  平均编码长度 = 最短平均编码长度 + 一个增量

  交叉熵在深度学习中广泛使用，衡量了测试集标签分布和模型预测分布之间的差异程度。

- 编码方法不一定完美时，平均编码长度相对于最小值的增加量（即上面那个增量）是相对熵。

**交叉熵 = 信息熵 + 相对熵**



现在要用IN8来编码FP32的信息， 同时要求编码后的差异要尽可能的小， 

KL_divergence(P, Q): = SUM(P[i] * log(P[i]/Q[i], i)

P = reference_distribution

Q = quantize_distribution

每一层的tensor的阈值T都是不同的

确定每一层T的过程称之为**校准 calibration **



為了找到合理的閾值， 也必須要有先驗知識， 也就是每一层在FP32精度下的激活值分布



1. FP32精度訓練好的模型 這個一般框架訓練出來的都是， 这个我们是知道的， 激活值分布如何得到？ 



NVIDIA官方建议是

选取一个子集为校准集， 这个校准集必须代表性以及多样性， 最好是验证集的一个子集， 不应该只是分类类别的一个小部分



1. 首先在校准集上进行FP32 inference推理
2. 对于网络的每一层 
   - 收集这一层的激活值，并做 直方图（histograms ），分成几个组别（bins）（官方给的一个说明使用的是2048组），分组是为了下面遍历 |T| 时，减少遍历次数；
   - 对于不同的 阈值 |T| 进行遍历，因为这里 |T|的取值肯定在 第128-2047 组之间，所以就选取每组的中间值进行遍历；
     - 选取使得 KL_divergence(ref_distr, quant_distr) 取得最小值的 |T|。



3. 返回一系列 |T|值， 每一层都有一个|T| 创建CalibrationTable



假设最终使得KL散度最小的|T|值是第200组的中间值， 那么就拿原来第0-200组的数值线性映射到0~128之间， 超出范围的直接映射到128





基本工作流程 from GTC2017 PPT

- You will need:
  - Model trained in FP32.
  - Calibration dataset.
- TensorRT will:
  - Run inference in FP32 on calibration dataset.
  - Collect required statistics.
  - Run calibration algorithm → optimal scaling factors.
  - Quantize FP32 weights → INT8.
  - Generate “CalibrationTable” and INT8 execution engine.





------



------

> ### TensorRT API 
>
> 该章节主要记录各个类的用法， 内容涵盖从创建engine到序列、反序列， 最后自定义plugin等

**用INetworkDefinition 构建网路层**

IBuilder * builder = create





**创建Engine**

```c++
virtual nvinfer1::ICudaEngine* nvinfer1::IBuilder::buildEngineWithConfig(
INetworkDefinition & 	network,IBuilderConfig & 	config )					
```



```c++
virtual nvinfer1::IBuilder::buildCudaEngine([nvinfer1::INetworkDefinition]
```

该方法用来创建Engine， 旧方法 官网说未来会淘汰



```
virtual void nvinfer1::IExecutionContext::setProfiler	(	IProfiler * 		)	
```



#### 反序列化Engine

该小节记录deserialize model等相关的类



##### deserializeCudaEngine

```c++
virtual nvinfer1::ICudaEngine* nvinfer1::IRuntime::deserializeCudaEngine	(	
 const void * 	blob,
std::size_t 	size,
IPluginFactory * 	pluginFactory 
)	
```

- Parameters

  - blob： The memory that holds the serialized engine.
  - size： The size of the memory.
  - plugin：FactoryThe plugin factory, if any plugins are used by the network, otherwise nullptr.

- Returns

  The engine, or nullptr if it could not be deserialized.



#### Plugin 相关

//TODO

------

<h1 align=center>Torch.onnx</h1>

该function为torch框架内建

其余框架如果需要转换为onnx, 则参考工程链接 https://github.com/onnx/onnx

官方教程 https://pytorch.org/docs/stable/onnx.html

> ### 基础使用

```python
import torch
import torchvision

dummy_input = torch.randn(10, 3, 224, 224, device='cuda')
model = torchvision.models.alexnet(pretrained=True).cuda()


input_names = [ "actual_input_1" ] + [ "learned_%d" % i for i in range(16) ]
output_names = [ "output1" ]

torch.onnx.export(model, dummy_input, "alexnet.onnx", verbose=True, input_names=input_names, output_names=output_names)
```

得到的 `alexnet.onnx` 是一个 protobuf 二值文件, 它包含所导出模型 ( 这里是 AlexNet )中网络架构和网络参数. 关键参数 `verbose=True` 会使导出过程中打印出的网络可读性较佳



你也可以使用 onnx 库来验证 输出的protobuf, 用 conda 安装 `onnx`

```
conda install -c conda-forge onnx
```

然后运行:

```python
import onnx

# 载入onnx模块
model = onnx.load("alexnet.onnx")

#检查IR是否良好
onnx.checker.check_model(model)

#输出一个图形的可读表示方式
onnx.helper.printable_graph(model.graph)
```



> ### 使用onnxruntime来验证exported model



如果以及安装onnxruntime, 则可以用来测试转好的模型

```python
import onnxruntime as ort

ort_session = ort.InferenceSession('alexnet.onnx')

outputs = ort_session.run(None, {'actual_input_1': np.random.randn(10, 3, 224, 224).astype(np.float32)})

print(outputs[0])
```

> ### Tracing and scripting

- Trace-based
  - 表示执行模型一次， 并且输出实际在模型中运行的op， 这表示如果模型为动态的， 输出将会依据输入的不同而有所改变， 而输入的数据形式最好不变

```python
import torch

# Trace-based only

class LoopModel(torch.nn.Module):
    def forward(self, x, y):
        for i in range(y):
            x = x + i
        return x

model = LoopModel()
dummy_input = torch.ones(2, 3, dtype=torch.long)
loop_count = torch.tensor(5, dtype=torch.long)

torch.onnx.export(model, (dummy_input, loop_count), 'loop.onnx', verbose=True)
```

```python
graph(%0 : Long(2, 3),
      %1 : Long()):
  %2 : Tensor = onnx::Constant[value={1}]()
  %3 : Tensor = onnx::Add(%0, %2)
  %4 : Tensor = onnx::Constant[value={2}]()
  %5 : Tensor = onnx::Add(%3, %4)
  %6 : Tensor = onnx::Constant[value={3}]()
  %7 : Tensor = onnx::Add(%5, %6)
  %8 : Tensor = onnx::Constant[value={4}]()
  %9 : Tensor = onnx::Add(%7, %8)
  return (%9)
```



- Script-based :
  - 表示输出的模型为ScrptModule形式的， 是一种TorchScript的数据结构， Torchscript则从pytorch code中创建出可序列化及可优化的模型

```python
@torch.jit.script
def loop(x, y):
    for i in range(int(y)):
        x = x + i
    return x

class LoopModel2(torch.nn.Module):
    def forward(self, x, y):
        return loop(x, y)

model = LoopModel2()
dummy_input = torch.ones(2, 3, dtype=torch.long)
loop_count = torch.tensor(5, dtype=torch.long)
torch.onnx.export(model, (dummy_input, loop_count), 'loop.onnx', verbose=True,
                  input_names=['input_data', 'loop_range'])
```



```python
graph(%input_data : Long(2, 3),
      %loop_range : Long()):
  %2 : Long() = onnx::Constant[value={1}](), scope: LoopModel2/loop
  %3 : Tensor = onnx::Cast[to=9](%2)
  %4 : Long(2, 3) = onnx::Loop(%loop_range, %3, %input_data), scope: LoopModel2/loop # custom_loop.py:240:5
    block0(%i.1 : Long(), %cond : bool, %x.6 : Long(2, 3)):
      %8 : Long(2, 3) = onnx::Add(%x.6, %i.1), scope: LoopModel2/loop # custom_loop.py:241:13
      %9 : Tensor = onnx::Cast[to=9](%2)
      -> (%9, %8)
  return (%4)
```

> ### Torch.onnx Functions

`Torch.onnx.export`

该function主要用来将model转换为onnx类型， 首先需要执行model来取得执行轨迹， 

```
torch.onnx.export(model, args, f, export_params=True, verbose=False, training=False, input_names=None, output_names=None, aten=False, export_raw_ir=False, operator_export_type=None, opset_version=None, _retain_param_name=True, do_constant_folding=False, example_outputs=None, strip_doc_string=True, dynamic_axes=None, keep_initializers_as_inputs=None)
```

- model ：要转换的model

- args ： 

- f : 就是输出的filename包含扩展名 例如test.onnx

- **export_params** : 默认开启， 将模型参数也输出， 如果设置为否， 则输出的模型为untrained

- verbose ： 如果开启， 则会输出被输出的trace的debug descrption

- training ： 一般不需要开启， 因为onnx 默认导向为输出 做inference only

- input_names :字符串列表， 默认为空列表， 用来指定graph 的input node

- output_names: 同上，相反

  其余查阅[https://pytorch.org/docs/stable/onnx.html?highlight=onnx%20export#torch.onnx.export](https://pytorch.org/docs/stable/onnx.html?highlight=onnx export#torch.onnx.export)

- **operator_export_type** : op输出的type

  - (*enum**,* *default OperatorExportTypes.ONNX*) –

    OperatorExportTypes.ONNX: all ops are exported as regular ONNX ops. OperatorExportTypes.ONNX_ATEN: all ops are exported as ATen ops. OperatorExportTypes.ONNX_ATEN_FALLBACK: if symbolic is missing, fall back on ATen op

    OperatorExportTypes.RAW: export raw ir.



> ### onnx support operation
>
> onnx目前支持的operation
>
> https://github.com/onnx/onnx/blob/master/docs/Operators.md



> ### Pytorch 实现自定义 operation， 并转换onnx
>
> https://blog.csdn.net/qq_33120609/article/details/99429967#pytorchop_9
>
> https://blog.csdn.net/qq_33120609/article/details/98850601



步骤如下

1. 自定义OP
2. 在自定义OP中加入symbolic





#### 自定义OP C++ 实现

以下 截取自 [pytorch 添加c实现的自定义](http://www.shijinglei.com/2019/12/21/pytorch-添加c实现的自定义op/)

pytorch已经基本实现了常见的各种op，然而，当想实现一个pytorch中没有的op时，有两种方式。一种方式是这个op可以由pytorch中已有的op进行组合而成，因此只需要使用python接口进行组合就可以了。反之，就必须使用c++或者cuda实现该op，然后添加到pytorch中。本文将介绍添加c++实现的自定义op，因为我还不会cuda : (

本文介绍使用python的setuptools将c++实现的op添加到pytorch中。首先要用c++实现定义的op。比如想实现一个op为 z=3x-y 。头文件为my_op.h

```c++
#include <torch/extension.h> //这一句是无论要实现任何op都必须添加的
#include <vector>

//前向传播
torch::Tensor my_op_forward(const torch::Tensor& x, const torch::Tensor& y);
//反向传播
std::vector<torch::Tensor> my_op_backward(const torch::Tensor& gradOutput);
```

源文件为my_op.cpp

```C++
#include "my_op.h"

torch::Tensor my_op_forward(const torch::Tensor& x,                             const torch::Tensor& y) {     
     AT_ASSERTM(x.sizes() == y.sizes(), "x must be the same size as y");
     torch::Tensor z = torch::zeros(x.sizes());
     z = 3 * x - y;
     return z; } 

std::vector<torch::Tensor> my_op_backward(const torch::Tensor& gradOutput) {
     torch::Tensor gradOutputX = 3 * gradOutput * torch::ones(gradOutput.sizes());
     torch::Tensor gradOutputY = -1 * gradOutput * torch::ones(gradOutput.sizes());
     return {gradOutputX, gradOutputY}; } 

// pybind11 绑定 
PYBIND11_MODULE(my_op_api, m) {
     m.def("forward", &my_op_forward, "MY_OP forward");
     m.def("backward", &my_op_backward, "MY_OP backward"); 
} 
```

其中最后的PYBIND11_MODULE是用来将C++函数绑定到python上的。其中第一个参数my_op_api为要生成的python模块名，以后import my_op_api就可以调用该op了。第二个参数固定为m
函数体中的两个语句分别是绑定前向传播与反向传播到实现的两个函数上。

然后编写setup.py，用来构建pytorch的c++扩展。

```
from setuptools import setup
from torch.utils.cpp_extension import BuildExtension, CppExtension

setup(name='my_op_api',
      version='0.l',
      ext_modules=[CppExtension('my_op_api', sources=['my_op.cpp'], extra_compile_args=['-std=c++11'])],
      cmdclass={'build_ext':BuildExtension})
```

其中，setup中的name以及CppExtension中第一个参数（也是name）要和PYBIND11_MODULE里设的模块名保持一致，这里都是my_op_api。CppExtension中的extra_compile_args=[‘-std=c++11’]是趟坑发现的，不加的话gcc可能会报n多错（pytorch是用c++11编译的，因此这里用gcc编译的时候也要使用c++11）

然后运行python setup.py install，如果没有问题的话，就生成了所需的python模块。可以从输出的信息看到该模块在所在python环境下的site-packages文件夹下，以.egg结尾。另外，在当前目录下会有3个文件夹生成，build、dist、my_op_api.egg-info，其中dist下也有.egg文件，可以发布到其它python环境。

然后就可以在python中import my_op_api进行调用扩展的op了。这里需要注意一点的是，在import 自定义的op之前，必须先import torch。 但是，这样的op和我们日常使用的还是不太一样，这时需要将它包装为pytorch中的函数和模块，以便我们像使用其它模块一样使用自定义的op。要包装为模块，首先包装成函数。

包装成函数，需要继承torch.autograd.Function。然后包装成模块，需要继承torch.nn.Module

```python
import torch
from torch.autograd import Function
from torch.nn import Module
import my_op_api

class MyOpFunction(Function):
    @staticmethod
    def forward(ctx, x, y):
        #如果有一些信息，需要在梯度反向传播时用到，可以使用ctx.save_for_backward()进行保存
        return my_op_api.forward(x, y)
    @staticmethod
    def backward(ctx, gradOutput):
        #如果在forward中保存了信息，可以使用ctx.saved_tensors取回
        grad_x, grad_y = my_op_api.backward(gradOutput)
        return grad_x, grad_y

class MyOpModule(Module):
    def __init__(self):
        super(MyOpModule, self).__init__()
    def forward(self, input_x, input_y):#只需要定义forward的函数就可以了
        return MyOpFunction.apply(input_x, input_y)
```

#### 在自定义OP中加入symbolic函数

```python
import torch
from torch.autograd import Function
from torch.nn import Module
import my_op_api

class MyOpFunction(Function):
  	@staticmethod
    def symbolic(g, x, y):
      	return g.op("myfunction", x, y)
        
        
    @staticmethod
    def forward(ctx, x, y):
        #如果有一些信息，需要在梯度反向传播时用到，可以使用ctx.save_for_backward()进行保存
        return my_op_api.forward(x, y)
    @staticmethod
    def backward(ctx, gradOutput):
        #如果在forward中保存了信息，可以使用ctx.saved_tensors取回
        grad_x, grad_y = my_op_api.backward(gradOutput)
        return grad_x, grad_y

class MyOpModule(Module):
    def __init__(self):
        super(MyOpModule, self).__init__()
    def forward(self, input_x, input_y):#只需要定义forward的函数就可以了
        return MyOpFunction.apply(input_x, input_y)
```







------

<h1 align=center>Libtorch</h1> 

pytorch libtorch 文档 https://pytorch.org/cppdocs/

pytorch libtorch 安装 https://pytorch.org/cppdocs/installing.html

//TODO 待新增

<h1 align=center>Onnx runtime</h1> 

onnxruntime 工程链接 https://github.com/microsoft/onnxruntime

ONNX Runtime是将 ONNX 模型部署到生产环境的跨平台高性能运行引擎。
适用于 Linux、Windows 和 Mac。编写C++，它还具有 C、Python 和C# api。 ONNX 运行时为所有 ONNX 规范提供支持，并与不同硬件（如 TensorRT 上的 NVidia Gpu）上的加速器集成





<h1 align=center>Onnx-TensorRT</h1>

onnx-tensorrt工程主要为解析onnx的model并且转换为tensorRT

需要先依照官方需求安装指定的TensorRT版本以及protobuf



比如你有onnx模型结构比较复杂， 需要更多的自定义op, 或者是从torch版本比较高转换的， 就需要依赖TensorRT7， 那么onnx-tensorrt就选择7.0的版本



截至2020年1月 支持到tensorrt7

工程链接 https://github.com/onnx/onnx-tensorrt

> ### Onnx-TensorRT 安装步骤

先确认环境， 示例中的环境为docker container 中， TensorRT版本为7



1. 依照以下官方步骤下载到本地

- git clone --recurse-submodules https://github.com/onnx/onnx-tensorrt.git

- `cd onnx-tensorrt`

- `mkdir build`

- `cmake .. -DTENSORRT_ROOT=<tensorrt_install_dir>`
  或是
  `cmake .. -DTENSORRT_ROOT=<tensorrt_install_dir> -DGPU_ARCHS="61" ` 

  例如:  `cmake .. -DTENSORRT_ROOT=/path/to/TensorRT-7.x.x.x -DGPU_ARCHS="61" `

  这个部分就是指定trt的路径联合构建编译

- `make -j8` 进行编译(记得是在onnx-tensorrt目录下执行)

- `sudo make install` 

- 安装完成后command指令输入 `onnx2trt` 确认一下指令是否可行



#### 中途遇见的问题

**问题1**

Tensorrt 装在DL的conda环境中， 但是要编译onnx的时候遇到 `/usr/bin/cmake: 没有那个文件或目录`

必须退出当前的conda 环境才能使用cmake 但是用cmake的时候又找不到tensorrt环境，因为装在conda dl的环境中



**问题2**

cmake的时候会找不到TensorRT library 

报错如下

```shell
-- Found TensorRT headers at /input/TensorRT-6.0.1.5/include
-- Find TensorRT libs at /input/TensorRT-6.0.1.5/lib/libnvinfer.so;/input/TensorRT-6.0.1.5/lib/libnvinfer_plugin.so;TENSORRT_LIBRARY_MYELIN-NOTFOUND
-- Could NOT find TENSORRT (missing: TENSORRT_LIBRARY)
ERRORCannot find TensorRT library.
CMake Error: The following variables are used in this project, but they are set to NOTFOUND.
Please set them or make sure they are set and tested correctly in the CMake files:
TENSORRT_LIBRARY_MYELIN
    linked by target "nvonnxparser_static" in directory /input/onnx-tensorrt
    linked by target "nvonnxparser" in directory /input/onnx-tensorrt

-- Configuring incomplete, errors occurred!
See also "/input/onnx-tensorrt/build/CMakeFiles/CMakeOutput.log".
See also "/input/onnx-tensorrt/build/CMakeFiles/CMakeError.log".
```



**解决2**

只是因为onnx1.6只支持tensorrt7的版本, 碰到这个问题是安装成6.0的TensorRT



**问题3**

make -j8 的时候 出现 onnx_utils.hpp文件的报错

报错如下

```
numeric_limits’ is not a member of ‘std’
```

表示出错的onnx_utils.hpp头文件没有包含

```
#include <stdexcept>
#include <limits>
```

**解决**

只要vim 进去加上两个头文件就可以



> ### onnx 转tensorrt 

主要是基本的auto转换方式



以下参考 https://github.com/onnx/onnx-tensorrt

使用onnx2trt指令 指定要转换的onnx 模型， -o 表示输出为 xxxxx.trt

```
onnx2trt my_model.onnx -o my_engine.trt
```

也可以将onnx 模型转换为可读txt文件

```
onnx2trt my_model.onnx -t my_model.onnx.txt
```

`-h` 可查看使用方法

```
onnx2trt -h
```





> ### onnx转tensorrt （自定义plugin）
>
> 以下参考 https://blog.csdn.net/qq_33120609/article/details/99429967#pytorchop_9

假设已经自定义了一个pytorch的DCNv2的op， 并且成功转换为onnx， 但因onnx转换成trt时还需要cuda的实现并且注册

注意一下流程实现环境为 **TensorRT 5.0 / onnx-tensorrt-5.0**

#### 流程

1. 确认onnx-tensorrt已经下载
2. 参考InstanceNormalization.cpp/.h ,写好自己DCN_v2.hpp和DCN_v2.cpp的实现。(同样可以参考FancyActivation,ResizeNearest等，都是官方写好的自定义op的示例，是op的逻辑)
3. 在builtin_op_importers.cpp中使用`DEFINE_BUILTIN_OP_IMPORTER`添加对自己注册Op的使用。
4. 在CMakeLists.txt中，`set(IMPORTER_SOURCES... `下面将自己的DCN_v2.cpp加进去。
5. 按照官方，重新编译自己的onnx-tensorrt， 然后拿自己输出的onnx文件测试自定义的op层是否可以正常读取



可以看到onnx-tensorrt 中的 这4个OP都是注册在onnx-tensorrt中， 并非原先在tensorrt中就有， 所以可以用一样的方式注册， Split,FancyActivation,InstanceNormalization,ResizeNearest

官方repo master原始码中的InstanceInstanceNormalization 继承 onnx2trt::PluginV2

```c++
class InstanceNormalizationPlugin final : public onnx2trt::PluginV2
```

PluginCreator的部分则是继承nvinfer1::IPluginCreator

```c++
class InstanceNormalizationPluginCreator : public nvinfer1::IPluginCreator
```



下面例子中为onnx-tensorrt 5.0, 继承的为Plugin而不是PluginV2

onnx-tensorrt5.1之后都是继承PluginV2

#### Plugin API 接口应用

1. 自定义DCNv2.hpp
   - 继承onnx2trt::Plugin
   - 定义参数(_in_channel, _out_channel_….)
   - 定义protected函数， 包含反序列各种参数
     - 定义getSerializationSize函数
     - 定义Serialize 序列化函数
   - 定义public函数
     - 定义初始化方法
     - 定义getPlugintype方法
     - 定义getNbOutputs
     - 定义initialize
     - 定义terminate
     - 定义enqueue
     - 定义getWorkspaceSize
     - 析构函数

```c++
#ifndef ONNX2TRT_DCNV2_H
#define ONNX2TRT_DCNV2_H

#pragma once

#include "plugin.hpp"
#include "serialize.hpp"
#include <cudnn.h>
#include <vector>
#include <cublas_v2.h>
#include <cuda.h>

class DCNv2Plugin final : public onnx2trt::Plugin {
    int _in_channel;
    int _out_channel;
    int _kernel_H;
    int _kernel_W;
    int _deformable_group;
    int _dilation;
    int _groups; // not use
    int _padding;
    int _stride;
    std::vector<float> _h_weight;
    std::vector<float> _h_bias;
    float* _d_weight;
    float* _d_bias;
    float* _d_ones;
    float *_d_columns;


    bool _initialized;

protected:
    void deserialize(void const* serialData, size_t serialLength) {
        deserializeBase(serialData, serialLength);
        deserialize_value(&serialData, &serialLength, &_in_channel);
        deserialize_value(&serialData, &serialLength, &_out_channel);
        deserialize_value(&serialData, &serialLength, &_kernel_H);
        deserialize_value(&serialData, &serialLength, &_kernel_W);
        deserialize_value(&serialData, &serialLength, &_deformable_group);
        deserialize_value(&serialData, &serialLength, &_dilation);
        deserialize_value(&serialData, &serialLength, &_groups);
        deserialize_value(&serialData, &serialLength, &_padding);
        deserialize_value(&serialData, &serialLength, &_stride);
        deserialize_value(&serialData, &serialLength, &_h_weight);
        deserialize_value(&serialData, &serialLength, &_h_bias);
    }
    size_t getSerializationSize() override {
        return (serialized_size(_in_channel) +
                serialized_size(_out_channel) +
                serialized_size(_kernel_H) +
                serialized_size(_kernel_W) +
                serialized_size(_deformable_group) +
                serialized_size(_dilation) +
                serialized_size(_groups) +
                serialized_size(_padding) +
                serialized_size(_stride) +
                serialized_size(_h_weight) +
                serialized_size(_h_bias)
               ) + getBaseSerializationSize();
    }
    void serialize(void *buffer) override {
        serializeBase(buffer);
        serialize_value(&buffer, _in_channel);
        serialize_value(&buffer, _out_channel);
        serialize_value(&buffer, _kernel_H);
        serialize_value(&buffer, _kernel_W);
        serialize_value(&buffer, _deformable_group);
        serialize_value(&buffer, _dilation);
        serialize_value(&buffer, _groups);
        serialize_value(&buffer, _padding);
        serialize_value(&buffer, _stride);
        serialize_value(&buffer, _h_weight);
        serialize_value(&buffer, _h_bias);
    }
public:
    DCNv2Plugin(int in_channel,
                int out_channel,
                int kernel_H,
                int kernel_W,
                int deformable_group,
                int dilation,
                int groups,
                int padding,
                int stride,
                nvinfer1::Weights const& weight,
                nvinfer1::Weights const& bias);

    DCNv2Plugin(void const* serialData, size_t serialLength) : _initialized(false) {
        this->deserialize(serialData, serialLength);
    }

    const char* getPluginType() const override { return "DCNv2"; }
    bool supportsFormat(nvinfer1::DataType type,
                        nvinfer1::PluginFormat format) const override;

    int getNbOutputs() const override { return 1; }
    nvinfer1::Dims getOutputDimensions(int index,
                                       const nvinfer1::Dims *inputDims,
                                       int nbInputs) override;

    int initialize() override;
    void terminate() override;
    int enqueue(int batchSize,
                const void *const *inputs, void **outputs,
                void *workspace, cudaStream_t stream) override;
    size_t getWorkspaceSize(int maxBatchSize) const override;
    ~DCNv2Plugin();
};


#endif //ONNX2TRT_DCNV2_H
```



2. 自定义DCNv2.cpp

```c++
#include "DCNv2.hpp"
#include "dcn_v2_im2col_cuda.h"

#define CHECK_CUDA(call) do {    \
  cudaError_t status = call; \
  if( status != cudaSuccess ) { \
    return status; \
  } \
} while(0)

cublasHandle_t blas_handle()
{
    static int init[16] = {0};
    static cublasHandle_t handle[16];
    int n = 0;
    cudaError_t status = cudaGetDevice(&n);
    if(!init[n]) {
        cublasCreate(&handle[n]);
        init[n] = 1;
    }
    return handle[n];
}
inline bool is_CHW(nvinfer1::Dims const& dims) {
    return (dims.nbDims == 3 &&
            dims.type[0] == nvinfer1::DimensionType::kCHANNEL &&
            dims.type[1] == nvinfer1::DimensionType::kSPATIAL &&
            dims.type[2] == nvinfer1::DimensionType::kSPATIAL);
}

DCNv2Plugin::DCNv2Plugin(int in_channel,
                         int out_channel,
                         int kernel_H,
                         int kernel_W,
                         int deformable_group,
                         int dilation,
                         int groups,
                         int padding,
                         int stride,
                         nvinfer1::Weights const &weight, nvinfer1::Weights const &bias):_in_channel(in_channel),
                        _out_channel(out_channel),_kernel_H(kernel_H),_kernel_W(kernel_W),_deformable_group(deformable_group),
                         _dilation(dilation),_groups(groups),_padding(padding),_stride(stride),_initialized(false){

    if (weight.type == nvinfer1::DataType::kFLOAT)
    {
        _h_weight.assign((float*)weight.values,(float*)weight.values+weight.count);
    } else { throw std::runtime_error("Unsupported  weight dtype");}

    if (bias.type == nvinfer1::DataType::kFLOAT)
    {
        _h_bias.assign((float*)bias.values,(float*)bias.values+bias.count);
    } else { throw std::runtime_error("Unsupported  bias dtype");}

}


int DCNv2Plugin::initialize() {
    if(_initialized) return 0;
    auto _output_dims = this->getOutputDimensions(0, &this->getInputDims(0), 3);
    assert(is_CHW(this->getInputDims(0)));
    assert(is_CHW(_output_dims));
    size_t ones_size = _output_dims.d[1]*_output_dims.d[2]* sizeof(float);
    size_t weight_size = _h_weight.size()* sizeof(float);
    size_t bias_size = _h_bias.size()* sizeof(float);
    float *ones_cpu = new float[ones_size/ sizeof(float)];
    for (int i = 0; i < ones_size/ sizeof(float); i++) {
        ones_cpu[i] = 1.0;
    }
    CHECK_CUDA(cudaMalloc((void**)&_d_columns, _in_channel * _kernel_H * _kernel_W * ones_size););
    CHECK_CUDA(cudaMalloc((void**)&_d_ones, ones_size));
    CHECK_CUDA(cudaMalloc((void**)&_d_weight, weight_size));
    CHECK_CUDA(cudaMalloc((void**)&_d_bias, bias_size));
    CHECK_CUDA(cudaMemcpy(_d_ones, ones_cpu, ones_size, cudaMemcpyHostToDevice));
    CHECK_CUDA(cudaMemcpy(_d_weight, _h_weight.data(), weight_size, cudaMemcpyHostToDevice));
    CHECK_CUDA(cudaMemcpy(_d_bias, _h_bias.data(), bias_size, cudaMemcpyHostToDevice));
    delete[] ones_cpu;
    _initialized = true;

    return 0;
}
void DCNv2Plugin::terminate() {
    if (!_initialized) {
        return;
    }
    cudaFree(_d_columns);
    cudaFree(_d_bias);
    cudaFree(_d_weight);
    cudaFree(_d_ones);
    _initialized = false;
}

DCNv2Plugin::~DCNv2Plugin() {
    terminate();
}
bool DCNv2Plugin::supportsFormat(nvinfer1::DataType type, nvinfer1::PluginFormat format) const {

    return (type == nvinfer1::DataType::kFLOAT);
}
nvinfer1::Dims DCNv2Plugin::getOutputDimensions(int index, const nvinfer1::Dims *inputDims, int nbInputs) {
    assert(index == 0);
    assert(inputDims);
    assert(nbInputs == 3);
    nvinfer1::Dims const& input = inputDims[0];
    assert(is_CHW(input));
    nvinfer1::Dims output;
    output.nbDims = input.nbDims;
    for( int d=0; d<input.nbDims; ++d ) {
        output.type[d] = input.type[d];
        output.d[d] = input.d[d];
    }
    output.d[0] = _out_channel;
    output.d[1] = (output.d[1] + 2 * _padding - (_dilation * (_kernel_H - 1) + 1)) / _stride + 1 ;
    output.d[2] = (output.d[2] + 2 * _padding - (_dilation * (_kernel_H - 1) + 1)) / _stride + 1 ;
    return output;
}
size_t DCNv2Plugin::getWorkspaceSize(int maxBatchSize) const {
    return 0;
}

int DCNv2Plugin::enqueue(int batchSize, const void *const *inputs, void **outputs, void *workspace,
                         cudaStream_t stream) {
    float alpha ,beta;
    int m, n, k;

    cublasHandle_t handle = blas_handle();
    const float* input = static_cast<const float *>(inputs[0]);
    const float* offset = static_cast<const float *>(inputs[1]);
    const float* mask = static_cast<const float *>(inputs[2]);
    float * output = static_cast<float *>(outputs[0]);
    nvinfer1::Dims input_dims = this->getInputDims(0);
    assert(batchSize==1);
    int h = input_dims.d[1];
    int w = input_dims.d[2];
    int height_out = (h + 2 * _padding - (_dilation * (_kernel_H - 1) + 1)) / _stride + 1;
    int width_out = (w + 2 * _padding - (_dilation * (_kernel_W - 1) + 1)) / _stride + 1;
    m = _out_channel;
    n = height_out * width_out;
    k = 1;
    alpha = 1.0;
    beta = 0.0;
    /// output  nxm
    /// ones    1xn  T ->> nx1
    /// bias    1xm
    /// ones x bias = nxm
    //  add bias
    cublasSgemm(handle,
                CUBLAS_OP_T, CUBLAS_OP_N,
                n, m, k,&alpha,
                _d_ones, k,
                _d_bias, k,&beta,
                output, n);
    // im2col (offset and mask)
    modulated_deformable_im2col_cuda(stream,input,offset,mask,
                                     1, _in_channel, h, w,
                                     height_out, width_out, _kernel_H, _kernel_W,
                                     _padding, _padding, _stride, _stride, _dilation, _dilation,
                                     _deformable_group, _d_columns);
    m = _out_channel;
    n = height_out * width_out;
    k = _in_channel * _kernel_H * _kernel_W;
    alpha = 1.0;
    beta = 1.0;
    // im2col conv
    cublasSgemm(handle,
                CUBLAS_OP_N, CUBLAS_OP_N,
                n, m, k,&alpha,
                _d_columns, n,
                _d_weight, k,
                &beta,
                output, n);
    return 0;
}
```







> onnx-tensorrt  5.1 继承PluginV2 范例

//TODO

#### PluginV2 API 接口应用

1. 自定义DCNv2.hpp
   - 继承onnx2trt::Plugin
   - 定义参数(_in_channel, _out_channel_….)
   - 定义protected函数， 包含反序列各种参数
     - 定义getSerializationSize函数
     - 定义Serialize 序列化函数
   - 定义public函数
     - 定义初始化方法
     - 定义getPlugintype方法
     - 定义getNbOutputs
     - 定义initialize
     - 定义terminate
     - 定义enqueue
     - 定义getWorkspaceSize
     - 析构函数 



------

<h1 align=center>Torch2Trt</h1>

官方项目地址 ： https://github.com/NVIDIA-AI-IOT/torch2trt



关于讨论找出不支持layer的issue

https://github.com/NVIDIA-AI-IOT/torch2trt/issues/130
