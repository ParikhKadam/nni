Experiment 配置（遗产）
====================================

这是上一个版本（V1）的实验配置规范。 目前仍然支持，但我们建议用户使用 `新版实验配置（V2） <../reference/experiment_config.rst>`_。

创建 Experiment 所需要的配置文件。 配置文件的路径会传入 ``nnictl`` 命令。
配置文件的格式为 YAML。
本文介绍了配置文件的内容，并提供了一些示例和模板。


* `Experiment 配置参考 <#experiment-config-reference>`__

  * `模板 <#template>`__
  * `说明 <#configuration-spec>`__

    * `authorName <#authorname>`__
    * `experimentName <#experimentname>`__
    * `trialConcurrency <#trialconcurrency>`__
    * `maxExecDuration <#maxexecduration>`__
    * `versionCheck <#versioncheck>`__
    * `debug <#debug>`__
    * `maxTrialNum <#maxtrialnum>`__
    * `trainingServicePlatform <#trainingserviceplatform>`__
    * `searchSpacePath <#searchspacepath>`__
    * `useAnnotation <#useannotation>`__
    * `multiThread <#multithread>`__
    * `nniManagerIp <#nnimanagerip>`__
    * `logDir <#logdir>`__
    * `logLevel <#loglevel>`__
    * `logCollection <#logcollection>`__
    * `tuner <#tuner>`__

      * `builtinTunerName <#builtintunername>`__
      * `codeDir <#codedir>`__
      * `classFileName <#classfilename>`__
      * `className <#classname>`__
      * `classArgs <#classargs>`__
      * `gpuIndices <#gpuindices>`__
      * `includeIntermediateResults <#includeintermediateresults>`__

    * `assessor <#assessor>`__

      * `builtinAssessorName <#builtinassessorname>`__
      * `codeDir <#codedir-1>`__
      * `classFileName <#classfilename-1>`__
      * `className <#classname-1>`__
      * `classArgs <#classargs-1>`__

    * `advisor <#advisor>`__

      * `builtinAdvisorName <#builtinadvisorname>`__
      * `codeDir <#codedir-2>`__
      * `classFileName <#classfilename-2>`__
      * `className <#classname-2>`__
      * `classArgs <#classargs-2>`__
      * `gpuIndices <#gpuindices-1>`__

    * `trial <#trial>`__
    * `localConfig <#localconfig>`__

      * `gpuIndices <#gpuindices-2>`__
      * `maxTrialNumPerGpu <#maxtrialnumpergpu>`__
      * `useActiveGpu <#useactivegpu>`__

    * `machineList <#machinelist>`__

      * `ip <#ip>`__
      * `port <#port>`__
      * `username <#username>`__
      * `passwd <#passwd>`__
      * `sshKeyPath <#sshkeypath>`__
      * `passphrase <#passphrase>`__
      * `gpuIndices <#gpuindices-3>`__
      * `maxTrialNumPerGpu <#maxtrialnumpergpu-1>`__
      * `useActiveGpu <#useactivegpu-1>`__
      * `pythonPath <#pythonPath>`__

    * `kubeflowConfig <#kubeflowconfig>`__

      * `operator <#operator>`__
      * `storage <#storage>`__
      * `nfs <#nfs>`__
      * `keyVault <#keyvault>`__
      * `azureStorage <#azurestorage>`__
      * `uploadRetryCount <#uploadretrycount>`__

    * `paiConfig <#paiconfig>`__

      * `userName <#username>`__
      * `password <#password>`__
      * `token <#token>`__
      * `host <#host>`__
      * `reuse <#reuse>`__

  * `示例 <#examples>`__

    * `本地模式 <#local-mode>`__
    * `远程模式 <#remote-mode>`__
    * `PAI 模式 <#pai-mode>`__
    * `Kubeflow 模式 <#kubeflow-mode>`__
    * `Kubeflow 中使用 Azure 存储 <#kubeflow-with-azure-storage>`__

模板
--------


* **简化版（不包含 Annotation（标记）和 Assessor）**

.. code-block:: yaml

   authorName:
   experimentName:
   trialConcurrency:
   maxExecDuration:
   maxTrialNum:
   # 可选项: local, remote, pai, kubeflow
   trainingServicePlatform:
   searchSpacePath:
   # 可选项: true, false, default: false
   useAnnotation:
   # 可选项: true, false, default: false
   multiThread:
   tuner:
     # 可选项: TPE, Random, Anneal, Evolution
     builtinTunerName:
     classArgs:
       # 可选项: maximize, minimize
       optimize_mode:
     gpuIndices:
   trial:
     command:
     codeDir:
     gpuNum:
   # 在本机模式下，machineList 可为空
   machineList:
     - ip:
       port:
       username:
       passwd:


* **使用 Assessor**

.. code-block:: yaml

   authorName:
   experimentName:
   trialConcurrency:
   maxExecDuration:
   maxTrialNum:
   # 可选项: local, remote, pai, kubeflow
   trainingServicePlatform:
   searchSpacePath:
   # 可选项: true, false, default: false
   useAnnotation:
   # 可选项: true, false, default: false
   multiThread:
   tuner:
     # 可选项: TPE, Random, Anneal, Evolution
     builtinTunerName:
     classArgs:
       # 可选项: maximize, minimize
       optimize_mode:
     gpuIndices:
   assessor:
     # 可选项: Medianstop
     builtinAssessorName:
     classArgs:
       # 可选项: maximize, minimize
       optimize_mode:
   trial:
     command:
     codeDir:
     gpuNum:
   # 在本机模式下，machineList 可为空
   machineList:
     - ip:
       port:
       username:
       passwd:


* **使用 Annotation**

.. code-block:: yaml

   authorName:
   experimentName:
   trialConcurrency:
   maxExecDuration:
   maxTrialNum:
   # 可选项: local, remote, pai, kubeflow
   trainingServicePlatform:
   # 可选项: true, false, default: false
   useAnnotation:
   # 可选项: true, false, default: false
   multiThread:
   tuner:
     # 可选项: TPE, Random, Anneal, Evolution
     builtinTunerName:
     classArgs:
       # 可选项: maximize, minimize
       optimize_mode:
     gpuIndices:
   assessor:
     # 可选项: Medianstop
     builtinAssessorName:
     classArgs:
       # 可选项: maximize, minimize
       optimize_mode:
   trial:
     command:
     codeDir:
     gpuNum:
   # 在本机模式下，machineList 可为空
   machineList:
     - ip:
       port:
       username:
       passwd:

说明
------------------

authorName
^^^^^^^^^^

必填。 字符串。

创建 Experiment 的作者的姓名。

*待定: 增加默认值。*

experimentName
^^^^^^^^^^^^^^

必填。 字符串。

创建的 Experiment 名称。

*待定: 增加默认值。*

trialConcurrency
^^^^^^^^^^^^^^^^

必填。 1 到 99999 之间的整数。

指定同时运行的 Trial 任务的最大数量。

注意：如果 trialGpuNum 大于空闲的 GPU 数量，并且并发的 Trial 任务数量还没达到 **trialConcurrency**，Trial 任务会被放入队列，等待分配 GPU 资源。

maxExecDuration
^^^^^^^^^^^^^^^

可选。 字符串。 默认值：999d。

**maxExecDuration** 指定实验的最大执行时间。 时间单位为 {**s**\ , **m**\ , **h**\ , **d**\ }，其分别表示 {*秒*\ , *分钟*\ , *小时*\ , *天*\ }。

注意：maxExecDuration 设置的是 Experiment 执行的时间，不是 Trial 的。 如果 Experiment 达到了设置的最大时间，Experiment 不会停止，但不会再启动新的 Trial 作业。

versionCheck
^^^^^^^^^^^^

可选。 布尔。 默认值：true。

NNI 会校验 remote, pai 和 Kubernetes 模式下 NNIManager 与 trialKeeper 进程的版本。 如果需要禁用版本校验，versionCheck 应设置为 false。

debug
^^^^^

可选。 布尔。 默认值：false。

调试模式会将 versionCheck 设置为 False，并将 logLevel 设置为 'debug'。

maxTrialNum
^^^^^^^^^^^

可选。 1 到 99999 之间的整数。 默认值：99999。

指定 NNI 创建的最大 Trial 任务数，包括成功和失败的任务。

trainingServicePlatform
^^^^^^^^^^^^^^^^^^^^^^^

必填。 字符串。

指定运行 Experiment 的平台，包括 **local**\ , **remote**\ , **pai**\ , **kubeflow**\ , **frameworkcontroller**。


* 
  **local** 在本机的 ubuntu 上运行 Experiment。

* 
  **remote** 将任务提交到远程的 ubuntu 上，必须用 **machineList** 来指定远程的 SSH 连接信息。

* 
  **pai**  提交到微软开源的 `OpenPAI <https://github.com/Microsoft/pai>`__ 上。 更多 OpenPAI 配置，参考 `PAI 模式指南 <../TrainingService/PaiMode.rst>`__。

* 
  **kubeflow** 提交任务到 `kubeflow <https://www.kubeflow.org/docs/about/kubeflow/>`__\ , NNI 支持基于 kubernetes 的 kubeflow，以及 `azure kubernetes <https://azure.microsoft.com/zh-cn/services/kubernetes-service/>`__。 详情参考 `Kubeflow Docs <../TrainingService/KubeflowMode.rst>`__。 详情参考 `Kubeflow Docs <../TrainingService/KubeflowMode.rst>`__。

* 
  **adl** 提交任务到 `AdaptDL <https://www.kubeflow.org/docs/about/kubeflow/>`__\ , NNI 支持 Kubernetes 集群上的 AdaptDL。 详情参考 `AdaptDL Docs <../TrainingService/AdaptDLMode.rst>`__。

* 
  TODO：解释 FrameworkController。

searchSpacePath
^^^^^^^^^^^^^^^

可选。 现有文件的路径。

指定搜索空间文件的路径，此文件必须在运行 nnictl 的本机。

仅在 ``useAnnotation=True`` 时，才不需要填写 **searchSpacePath**。

useAnnotation
^^^^^^^^^^^^^

可选。 布尔。 默认值：false。

使用 Annotation 分析 Trial 代码并生成搜索空间。

注意: 如果设置了 **useAnnotation=True**，searchSpacePath 字段必须被删除。

multiThread
^^^^^^^^^^^

可选。 布尔。 默认值：false。

为 Dispatcher 启用多线程模式。 如果启用了 multiThread，Dispatcher 将启动一个线程来处理来自 NNI 管理器的每个命令。

nniManagerIp
^^^^^^^^^^^^

可选。 字符串。 默认值：eth0 设备的 IP。

设置运行 NNI 管理器进程的计算机的 IP 地址。 此字段为可选项，如果没有设置，则会使用 eth0 的 IP 地址。

注意: 可在 NNI 管理器机器上运行 ifconfig 来检查 eth0 是否存在。 如果没有，建议显式设置 **nniManagerIp**。

logDir
^^^^^^

可选。 目录的路径。 默认：``<user home directory>/nni-experiments``。

配置目录以存储 Experiment 的日志和数据。

logLevel
^^^^^^^^

可选。 字符串。 默认值: ``info``。

设置 Experiment 的日志级别。 可设置的日志级别包括：``trace``\ , ``debug``\ , ``info``\ , ``warning``\ , ``error``\ , ``fatal``。

logCollection
^^^^^^^^^^^^^

可选。 ``http`` 或者 ``none``。 默认值：``none``。

设置在remote、pai、kubeflow、frameworkcontroller 平台中收集日志的方式。 日志支持两种设置，一种是通过 ``http``，让 Trial 将日志通过 POST 方法发回日志，这种方法会减慢 trialKeeper 的速度。 另一种方法是 ``none``，Trial 不将日志回传回来，仅仅回传 Job 的指标。 如果日志较大，可将此参数设置为 ``none``。

tuner
^^^^^

必填。

指定了 Experiment 的 Tuner 算法。有两种方法可设置 Tuner。 一种方法是使用 NNI SDK 提供的内置 Tuner，在这种情况下，需要设置 **builtinTunerName** 和 **classArgs**。 如果使用定制 Tuner，则为必需。 相对于 **codeDir** 的文件路径。 *必须选择其中的一种方式。*

builtinTunerName
^^^^^^^^^^^^^^^^

如果使用内置 Tuner，则为必需。 字符串。

指定系统 Tuner 的名称, NNI SDK 提供的各种 Tuner 的 `说明 <../Tuner/BuiltinTuner.rst>`__。

codeDir
^^^^^^^

如果使用定制 Tuner，则为必需。 相对于配置文件位置的路径。

指定 Tuner 代码的目录。

classFileName
^^^^^^^^^^^^^

如果使用定制 Tuner，则为必需。 相对于 **codeDir** 的文件路径。

指定 Tuner 文件的名称。

className
^^^^^^^^^

如果使用定制 Tuner，则为必需。 字符串。

指定 Tuner 的名称。

classArgs
^^^^^^^^^

可选。 键值对。 默认值：空。

指定 Tuner 算法的参数。 参考 `此文件 <../Tuner/BuiltinTuner.rst>`__ 来了解内置 Tuner 的配置参数。

gpuIndices
^^^^^^^^^^

可选。 字符串。 默认值：空。

指定 Tuner 进程可以使用的 GPU。 可以指定单个或多个 GPU 索引。 多个 GPU 索引用逗号 ``,`` 分隔。 例如, ``1``\ , 或 ``0,1,3``. 如果未设置该字段，则 Tuner 将找不到 GPU（设置 ``CUDA_VISIBLE_DEVICES`` 成空字符串）。

includeIntermediateResults
^^^^^^^^^^^^^^^^^^^^^^^^^^

可选。 布尔。 默认值：false。

如果 **includeIntermediateResults** 为 true，最后一个 Assessor 的中间结果会被发送给 Tuner 作为最终结果。

assessor
^^^^^^^^

指定 Assessor 算法以运行 Experiment。 与 Tuner 类似，有两种设置 Assessor 的方法。 一种方法是使用 NNI SDK 提供的 Assessor。 必填字段：builtinAssessorName 和 classArgs。 另一种方法，是使用用户自定义的 Assessor，需要设置 **codeDirectory**\ , **classFileName**\ , **className** 和 **classArgs**。 *必须选择其中的一种方式。*

默认情况下，未启用任何 Assessor。

builtinAssessorName
^^^^^^^^^^^^^^^^^^^

如果使用内置 Assessor，则为必需。 字符串。

指定内置 Assessor 的名称，NNI SDK 提供的 Assessor 可参考 `这里 <../Assessor/BuiltinAssessor.rst>`__。

codeDir
^^^^^^^

如果使用定制 Assessor，则为必需。 相对于配置文件位置的路径。

指定 Assessor 代码的目录。

classFileName
^^^^^^^^^^^^^

如果使用定制 Assessor，则为必需。 相对于 **codeDir** 的文件路径。

指定 Assessor 文件的名称。

className
^^^^^^^^^

如果使用定制 Assessor，则为必需。 字符串。

指定 Assessor 类的名称。

classArgs
^^^^^^^^^

可选。 键值对。 默认值：空。

指定 Assessor 算法的参数。

Advisor
^^^^^^^

可选。

指定 Experiment 中的 Advisor 算法。 与 Tuner 和 Assessor 类似，有两种指定 Advisor 的方法。 一种方法是使用 SDK 提供的 Advisor ，需要设置 **builtinAdvisorName** 和 **classArgs**。 另一种方法，是使用用户自定义的 Advisor ，需要设置 **codeDirectory**\ , **classFileName**\ , **className** 和 **classArgs**。

启用 Advisor 后，将忽略 Tuner 和 Advisor 的设置。

builtinAdvisorName
^^^^^^^^^^^^^^^^^^

指定内置 Advisor 的名称。 指定内置 Advisor 的名称。 NNI SDK 提供了 `BOHB <../Tuner/BohbAdvisor.rst>`__ 和 `Hyperband <../Tuner/HyperbandAdvisor.rst>`__ 。

codeDir
^^^^^^^

如果使用定制 Advisor，则为必需。 相对于配置文件位置的路径。

指定 Advisor 代码的目录。

classFileName
^^^^^^^^^^^^^

如果使用定制 Advisor，则为必需。 相对于 **codeDir** 的文件路径。

指定 Advisor 文件的名称。

className
^^^^^^^^^

如果使用定制 Advisor，则为必需。 字符串。

指定 Advisor 类的名称。

classArgs
^^^^^^^^^

可选。 键值对。 默认值：空。

指定 Advisor 的参数。

gpuIndices
^^^^^^^^^^

可选。 字符串。 默认值：空。

指定可以使用的 GPU。 可以指定单个或多个 GPU 索引。 多个 GPU 索引用逗号 ``,`` 分隔。 例如, ``1``\ , 或 ``0,1,3``. 如果未设置该字段，则 Tuner 将找不到 GPU（设置 ``CUDA_VISIBLE_DEVICES`` 成空字符串）。

trial
^^^^^

必填。 键值对。

在 local 和 remote 模式下，需要以下键。


* 
  **command**：必需字符串。 指定运行 Trial 的命令。

* 
  **codeDir**：必需字符串。 指定 Trial 文件的目录。 此目录将在 remote 模式下自动上传。

* 
  **gpuNum**：可选、整数。 指定了运行 Trial 进程的 GPU 数量。 默认值为 0。

在 PAI 模式下，需要以下键。


* 
  **command**：必需字符串。 指定运行 Trial 的命令。

* 
  **codeDir**：必需字符串。 指定 Trial 文件的目录。 目录中的文件将在 PAI 模式下上传。

* 
  **gpuNum**：必需、整数。 指定了运行 Trial 进程的 GPU 数量。 默认值为 0。

* 
  **cpuNum**：必需、整数。 指定要在 OpenPAI 容器中使用的 cpu 数。

* 
  **memoryMB**：必需、整数。 设置要在 OpenPAI 容器中使用的内存大小，以兆字节为单位。

* 
  **image**：必需字符串。 设置要在 OpenPAI 中使用的 Docker 映像。

* 
  **authFile**：可选、字符串。 用于提供 Docker 注册，用于为 OpenPAI 中的映像拉取请求进行身份验证。 `参考 <https://github.com/microsoft/pai/blob/2ea69b45faa018662bc164ed7733f6fdbb4c42b3/docs/faq.rst#q-how-to-use-private-docker-registry-job-image-when-submitting-an-openpai-job>`__.

* 
  **shmMB**：可选、整数。 容器的共享内存大小。

* 
  **portList**\ : ``label``\ , ``beginAt``\ , ``portNumber`` 的键值对 list。 参考 `OpenPAI 教程 <https://github.com/microsoft/pai/blob/master/docs/job_tutorial.rst>`__ 。

.. cannot find `Reference <https://github.com/microsoft/pai/blob/2ea69b45faa018662bc164ed7733f6fdbb4c42b3/docs/faq.rst#q-how-to-use-private-docker-registry-job-image-when-submitting-an-openpai-job>`__  and `job tutorial of PAI <https://github.com/microsoft/pai/blob/master/docs/job_tutorial.rst>`__ 

在 Kubeflow 模式下，需要以下键。


* 
  **codeDir**：指定了代码文件的本机路径。

* 
  **ps**：Kubeflow 的 tensorflow-operator 的可选配置，包括：


  * 
    **replicas**：``ps`` 角色的副本数量。

  * 
    **command**：**ps** 容器运行的脚本。

  * 
    **gpuNum**：在 **ps** 容器中使用的 GPU 数量。

  * 
    **cpuNum**：在 **ps** 容器中使用的 CPU 数量。

  * 
    **memoryMB**：容器的内存大小。

  * 
    **image**：在 **ps** 中使用的 Docker 映像。

* 
  **worker**：是 Kubeflow 的 tensorflow-operator 的可选配置。


  * 
    **replicas**：**worker** 角色的副本数量。

  * 
    **command**：**worker** 容器运行的脚本。

  * 
    **gpuNum**：在 **worker** 容器中使用的 GPU 数量。

  * 
    **cpuNum**：在 **worker** 容器中使用的 CPU 数量。

  * 
    **memoryMB**：容器的内存大小。

  * 
    **image**：在 **worker** 中使用的 Docker 映像。

localConfig
^^^^^^^^^^^

本机模式下可选。 键值对。

仅在 **trainingServicePlatform** 设为 **local** 时有效，否则，配置文件中不应该有 **localConfig** 部分。

gpuIndices
^^^^^^^^^^

可选。 字符串。 默认值：none。

用于指定特定的 GPU。设置此值后，只有指定的 GPU 会被用来运行 Trial 任务。 可以指定单个或多个 GPU 索引。 多个 GPU 索引，应用逗号（``,``）分隔，如 ``1`` 或  ``0,1,3``。 默认情况下，将使用所有可用的 GPU。

maxTrialNumPerGpu
^^^^^^^^^^^^^^^^^

可选。 整数。 默认值： 1。

用于指定 GPU 设备上的最大并发 Trial 的数量。

useActiveGpu
^^^^^^^^^^^^

可选。 布尔。 默认值：false。

用于指定 GPU 上存在其他进程时是否使用此 GPU。 默认情况下，NNI 仅在 GPU 中没有其他活动进程时才使用 GPU。 如果 **useActiveGpu** 设置为 true，则 NNI 无论某 GPU 是否有其它进程，都将使用它。 此字段不适用于 Windows 版的 NNI。

machineList
^^^^^^^^^^^

在 remote 模式下必需。 具有以下键的键值对的列表。

ip
^^

必填。 可从当前计算机访问的 IP 地址或主机名。

远程计算机的 IP 地址或主机名。

port
^^^^

可选。 整数。 有效端口。 默认值： 22。

用于连接计算机的 SSH 端口。

username
^^^^^^^^

使用用户名/密码进行身份验证时是必需的。 字符串。

远程计算机的帐户。

passwd
^^^^^^

使用用户名/密码进行身份验证时是必需的。 字符串。

指定帐户的密码。

sshKeyPath
^^^^^^^^^^

如果使用 SSH 密钥进行身份验证，则为必需。 私钥文件的路径。

如果用户使用 SSH 密钥登录远程计算机，**sshKeyPath** 应是有效的 SSH 密钥文件路径。

*注意：如果同时设置了 passwd 和 sshKeyPath，NNI 会首先使用 passwd。*

passphrase
^^^^^^^^^^

可选。 字符串。

用于保护 SSH 密钥，如果用户没有密码，可为空。

gpuIndices
^^^^^^^^^^

可选。 字符串。 默认值：none。

用于指定特定的 GPU。设置此值后，只有指定的 GPU 会被用来运行 Trial 任务。 可以指定单个或多个 GPU 索引。 多个 GPU 索引，应用逗号（``,``）分隔，如 ``1`` 或  ``0,1,3``。 默认情况下，将使用所有可用的 GPU。

maxTrialNumPerGpu
^^^^^^^^^^^^^^^^^

可选。 整数。 默认值： 1。

用于指定 GPU 设备上的最大并发 Trial 的数量。

useActiveGpu
^^^^^^^^^^^^

可选。 布尔。 默认值：false。

用于指定 GPU 上存在其他进程时是否使用此 GPU。 默认情况下，NNI 仅在 GPU 中没有其他活动进程时才使用 GPU。 如果 **useActiveGpu** 设置为 true，则 NNI 无论某 GPU 是否有其它进程，都将使用它。 此字段不适用于 Windows 版的 NNI。

pythonPath
^^^^^^^^^^

可选。 字符串。

用户可以通过设置 **pythonPath**，在远程机器上配置 Python 环境。

remoteConfig
^^^^^^^^^^^^

在 remote 模式下可选。 用户可以在 ``machineList`` 字段中设置每台机器的信息，并在该字段中设置远程模式的全局配置。

reuse
^^^^^

可选。 布尔。 默认值：``false``。 这是试用中的功能。

如果为 true，NNI 会重用远程作业，在其中运行尽可能多的 Trial。 这样可以节省创建新作业的时间。 用户需要确保同一作业中的每个 Trial 相互独立，例如，要避免从之前的 Trial 中读取检查点。 

kubeflowConfig
^^^^^^^^^^^^^^

operator
^^^^^^^^

必填。 字符串。 必须是 ``tf-operator`` 或 ``pytorch-operator``。

指定要使用的 Kubeflow 运算符，当前版本中 NNI 支持 ``tf-operator``。

storage
^^^^^^^

可选。 字符串。 默认值 ``nfs``。

指定 Kubeflow 的存储类型，包括 ``nfs`` 和 ``azureStorage``。

nfs
^^^

如果使用 nfs，则必需。 键值对。


* 
  **server** 是 NFS 服务器的地址。

* 
  **path** 是 NFS 挂载的路径。

keyVault
^^^^^^^^

如果使用 Azure 存储，则必需。 键值对。

将 **keyVault** 设置为 Azure 存储帐户的私钥。 参考 `此文档 <https://docs.microsoft.com/zh-cn/azure/key-vault/key-vault-manage-with-cli2>`__ 。


* 
  **vaultName** 是 az 命令中 ``--vault-name`` 的值。

* 
  **name** 是 az 命令中 ``--name`` 的值。

azureStorage
^^^^^^^^^^^^

如果使用 Azure 存储，则必需。 键值对。

设置 Azure 存储帐户以存储代码文件。


* 
  **accountName** 是 Azure 存储账户的名称。

* 
  **azureShare** 是 Azure 文件存储的共享参数。

uploadRetryCount
^^^^^^^^^^^^^^^^

如果使用 Azure 存储，则必需。 1 到 99999 之间的整数。

如果上传文件至 Azure Storage 失败，NNI 会重试。此字段指定了重试的次数。

paiConfig
^^^^^^^^^

userName
^^^^^^^^

必填。 字符串。

OpenPAI 帐户的用户名。

password
^^^^^^^^

如果使用密码身份验证，则需要。 字符串。

 OpenPAI 帐户的密码。

token
^^^^^

如果使用令牌（token）身份验证，则需要。 字符串。

可以从 OpenPAI 门户检索的个人访问令牌。

host
^^^^

必填。 字符串。

 OpenPAI 的 IP 地址。

reuse
^^^^^

可选。 布尔。 默认值：``false``。 这是试用中的功能。

如果为 true，NNI 会重用 OpenPAI 作业，在其中运行尽可能多的 Trial。 这样可以节省创建新作业的时间。 用户需要确保同一作业中的每个 Trial 相互独立，例如，要避免从之前的 Trial 中读取检查点。

sharedStorage
^^^^^^^^^^^^^

storageType
^^^^^^^^^^^

必填。 字符串。

存储类型，支持 ``NFS`` 和 ``AzureBlob``。

localMountPoint
^^^^^^^^^^^^^^^

必填。 字符串。

已经或将要在本地挂载存储的绝对路径。 如果路径不存在，则会自动创建。 建议使用绝对路径。 =========================== ``/tmp/nni-shared-storage``

remoteMountPoint
^^^^^^^^^^^^^^^^

必填。 字符串。

远程挂载存储的绝对路径。 如果路径不存在，则会自动创建。 请注意，如果使用 AzureBlob，该目录必须为空。 建议使用相对路径。 即， ``./nni-shared-storage``

localMounted
^^^^^^^^^^^^

必填。 字符串。

``usermount``、``nnimount`` 和 ``nomount`` 其中之一。 ``usermount`` 表示已经在 localMountPoint 上挂载了此存储。 ``nnimount`` 表示 nni 将尝试在 localMountPoint 上挂载此存储。 ``nomount`` 表示存储不会挂载在本地机器上，将在未来支持部分存储。 ``usermount`` 表示已经在 localMountPoint 上挂载了此存储。 ``nnimount`` 表示 nni 将尝试在 localMountPoint 上挂载此存储。 ``nomount`` 表示存储不会挂载在本地机器上，将在未来支持部分存储。

nfsServer
^^^^^^^^^

可选。 字符串。

如果使用 NFS 存储，则必需。 NFS 服务器的 host。 如果使用 NFS 存储，则必需。 NFS 服务器的导出目录。

exportedDirectory
^^^^^^^^^^^^^^^^^

可选。 字符串。

如果使用 NFS 存储，则必需。 NFS 服务器的导出目录。

storageAccountName
^^^^^^^^^^^^^^^^^^

可选。 字符串。

如果使用 AzureBlob 存储，则必需。 AzureBlob 容器名。 如果使用 AzureBlob 存储且 ``resourceGroupName`` 未设置，则必需。 Azure 存储账户密钥。

storageAccountKey
^^^^^^^^^^^^^^^^^

可选。 字符串。

如果使用 AzureBlob 存储且 ``storageAccountKey`` 未设置，则必需。 AzureBlob 容器所属的资源组。 Azure 存储账户密钥。

resourceGroupName
^^^^^^^^^^^^^^^^^

可选。 字符串。

如果使用 Azure 存储，则必需。 1 到 99999 之间的整数。 AzureBlob 容器所属的资源组。

containerName
^^^^^^^^^^^^^

可选。 字符串。

如果使用 AzureBlob 存储，则必需。 AzureBlob 容器名。

示例
--------

本机模式
^^^^^^^^^^

如果要在本机运行 Trial 任务，并使用标记来生成搜索空间，可参考下列配置：

.. code-block:: yaml

     authorName: test
     experimentName: test_experiment
     trialConcurrency: 3
     maxExecDuration: 1h
     maxTrialNum: 10
     # 可选项：local, remote, pai, kubeflow
     trainingServicePlatform: local
     # 可选项：true, false
     useAnnotation: true
     tuner:
       # 可选项：TPE, Random, Anneal, Evolution
       builtinTunerName: TPE
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     trial:
       command: python3 mnist.py
       codeDir: /nni/mnist
       gpuNum: 0

增加 Assessor 配置

.. code-block:: yaml

     authorName: test
     experimentName: test_experiment
     trialConcurrency: 3
     maxExecDuration: 1h
     maxTrialNum: 10
     # 可选项：local, remote, pai, kubeflow
     trainingServicePlatform: local
     searchSpacePath: /nni/search_space.json
     # 可选项：true, false
     useAnnotation: false
     tuner:
       # 可选项：TPE, Random, Anneal, Evolution
       builtinTunerName: TPE
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     assessor:
       # 可选项: Medianstop
       builtinAssessorName: Medianstop
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     trial:
       command: python3 mnist.py
       codeDir: /nni/mnist
       gpuNum: 0

或者可以指定自定义的 Tuner 和 Assessor：

.. code-block:: yaml

     authorName: test
     experimentName: test_experiment
     trialConcurrency: 3
     maxExecDuration: 1h
     maxTrialNum: 10
     # 可选项：local, remote, pai, kubeflow
     trainingServicePlatform: local
     searchSpacePath: /nni/search_space.json
     # 可选项：true, false
     useAnnotation: false
     tuner:
       codeDir: /nni/tuner
       classFileName: mytuner.py
       className: MyTuner
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     assessor:
       codeDir: /nni/assessor
       classFileName: myassessor.py
       className: MyAssessor
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     trial:
       command: python3 mnist.py
       codeDir: /nni/mnist
       gpuNum: 0

远程模式
^^^^^^^^^^^

如果要在远程服务器上运行 Trial 任务，需要增加服务器信息：

.. code-block:: yaml

     authorName: test
     experimentName: test_experiment
     trialConcurrency: 3
     maxExecDuration: 1h
     maxTrialNum: 10
     # 可选项：local, remote, pai, kubeflow
     trainingServicePlatform: remote
     searchSpacePath: /nni/search_space.json
     # 可选项：true, false
     useAnnotation: false
     tuner:
       # 可选项：TPE, Random, Anneal, Evolution
       builtinTunerName: TPE
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     trial:
       command: python3 mnist.py
       codeDir: /nni/mnist
       gpuNum: 0
     # 如果是本地平台的话，machineList 可以为空
     machineList:
       - ip: 10.10.10.10
         port: 22
         username: test
         passwd: test
       - ip: 10.10.10.11
         port: 22
         username: test
         passwd: test
       - ip: 10.10.10.12
         port: 22
         username: test
         sshKeyPath: /nni/sshkey
         passphrase: qwert
         # 以下是特定 python 环境的一个示例
         pythonPath: ${replace_to_python_environment_path_in_your_remote_machine}

PAI 模式
^^^^^^^^

.. code-block:: yaml

     authorName: test
     experimentName: nni_test1
     trialConcurrency: 1
     maxExecDuration:500h
     maxTrialNum: 1
     # 可选项：local, remote, pai, kubeflow
     trainingServicePlatform: pai
     searchSpacePath: search_space.json
     # 可选项：true, false
     useAnnotation: false
     tuner:
       # 可选项: TPE, Random, Anneal, Evolution, BatchTuner
       # SMAC (SMAC 应该通过 nnictl 安装)
       builtinTunerName: TPE
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     trial:
       command: python3 main.py
       codeDir: .
       gpuNum: 4
       cpuNum: 2
       memoryMB: 10000
       # 在 OpenPAI 上运行 NNI 的 Docker 映像
       image: msranni/nni:latest
     paiConfig:
       # 登录 OpenPAI 的用户名
       userName: test
       # 登录 OpenPAI 的密码
       passWord: test
       # OpenPAI 的 RestFUL 服务器地址
       host: 10.10.10.10

Kubeflow 模式
^^^^^^^^^^^^^^^^^^^^^^^^^^

  使用 NFS 存储。

.. code-block:: yaml

     authorName: default
     experimentName: example_mni
     trialConcurrency: 1
     maxExecDuration: 1h
     maxTrialNum: 1
     # 可选项：local, remote, pai, kubeflow
     trainingServicePlatform: kubeflow
     searchSpacePath: search_space.json
     # 可选项：true, false
     useAnnotation: false
     tuner:
       # 可选项：TPE, Random, Anneal, Evolution
       builtinTunerName: TPE
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     trial:
       codeDir: .
       worker:
         replicas: 1
         command: python3 mnist.py
         gpuNum: 0
         cpuNum: 1
         memoryMB: 8192
         image: msranni/nni:latest
     kubeflowConfig:
       operator: tf-operator
       nfs:
         server: 10.10.10.10
         path: /var/nfs/general

Kubeflow 中使用 Azure 存储
^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: yaml

     authorName: default
     experimentName: example_mni
     trialConcurrency: 1
     maxExecDuration: 1h
     maxTrialNum: 1
     # 可选项：local, remote, pai, kubeflow
     trainingServicePlatform: kubeflow
     searchSpacePath: search_space.json
     # 可选项：true, false
     useAnnotation: false
     # nniManagerIp: 10.10.10.10
     tuner:
       # 可选项：TPE, Random, Anneal, Evolution
       builtinTunerName: TPE
       classArgs:
         # 可选项: maximize, minimize
         optimize_mode: maximize
     assessor:
       builtinAssessorName: Medianstop
       classArgs:
         optimize_mode: maximize
     trial:
       codeDir: .
       worker:
         replicas: 1
         command: python3 mnist.py
         gpuNum: 0
         cpuNum: 1
         memoryMB: 4096
         image: msranni/nni:latest
     kubeflowConfig:
       operator: tf-operator
       keyVault:
         vaultName: Contoso-Vault
         name: AzureStorageAccountKey
       azureStorage:
         accountName: storage
         azureShare: share01
