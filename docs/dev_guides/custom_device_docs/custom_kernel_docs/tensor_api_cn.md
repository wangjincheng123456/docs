# Tensor API

飞桨发布多种Tensor，基类均为`TensorBase`，这里列举常用的`DenseTensor` API，`TensorBase`与其它Tensor类型请参照文后链接。

## DenseTensor

`DenseTensor`中的所有元素数据存储在连续内存中，请参照[dense_tensor.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/dense_tensor.h)

```c++
  // 构造DenseTensor并分配内存
  // 参数：a - Allocator指针类型
  //      meta - DenseTensorMeta对象
  // 返回：None
  DenseTensor(Allocator* a, const DenseTensorMeta& meta);

  // 构造DenseTensor并分配内存
  // 参数：a - Allocator指针类型
  //      meta - DenseTensorMeta移动对象
  // 返回：None
  DenseTensor(Allocator* a, DenseTensorMeta&& meta);

  // 构造DenseTensor并分配内存
  // 参数：holder - Allocation共享指针类型
  //      meta - DenseTensorMeta移动对象
  // 返回：None
  DenseTensor(const std::shared_ptr<phi::Allocation>& holder,
              const DenseTensorMeta& meta);

  // 移动构造函数
  // 参数：other - DenseTensor移动对象
  // 返回：None
  DenseTensor(DenseTensor&& other) = default;

  // 拷贝构造函数
  // 参数：other - DenseTensor对象
  // 返回：None
  DenseTensor(const DenseTensor& other);

  // 赋值操作
  // 参数：other - DenseTensor对象
  // 返回：DenseTensor对象
  DenseTensor& operator=(const DenseTensor& other);

  // 移动赋值操作
  // 参数：other - DenseTensor对象
  // 返回：DenseTensor对象
  DenseTensor& operator=(DenseTensor&& other);

  // 无参构造函数
  DenseTensor();

  // 析构函数
  virtual ~DenseTensor() = default;

  // 获取类名，静态函数
  // 参数：None
  // 返回：字符串指针
  static const char* name();

  // 获取Tensor中元素数量
  // 参数：None
  // 返回：int64_t类型变量
  int64_t numel() const override;

  // 获取Tensor的dims信息
  // 参数：None
  // 返回：DDim对象
  const DDim& dims() const noexcept override;

  // 获取Tensor的lod信息
  // 参数：None
  // 返回：LoD对象
  const LoD& lod() const noexcept;

  // 获取Tensor的数据类型信息
  // 参数：None
  // 返回：DataType类型变量
  DataType dtype() const noexcept override;

  // 获取Tensor的内存布局信息
  // 参数：None
  // 返回：DataLayout类型变量
  DataLayout layout() const noexcept override;

  // 获取Tensor的Place信息
  // 参数：None
  // 返回：Place类型变量
  const Place& place() const override;

  // 获取Tensor的meta信息
  // 参数：None
  // 返回：DenseTensorMeta对象
  const DenseTensorMeta& meta() const noexcept;

  // 设置Tensor的meta信息
  // 参数：meta - DenseTensorMeta移动对象
  // 返回：None
  void set_meta(DenseTensorMeta&& meta);

  // 设置Tensor的meta信息
  // 参数：meta - DenseTensorMeta对象
  // 返回：None
  void set_meta(const DenseTensorMeta& meta);

  // 检查Tensor的meta信息是否有效
  // 参数：None
  // 返回：bool类型变量
  bool valid() const noexcept override;

  // 检查Tensor的是否被初始化
  // 参数：None
  // 返回：bool类型变量
  bool initialized() const override;

  // 为Tensor分配内存
  // 参数：allocator - Allocator类型指针
  //      dtype - DataType变量
  //      requested_size - size_t类型变量
  // 返回：void*类型指针
  void* AllocateFrom(Allocator* allocator,
                     DataType dtype,
                     size_t requested_size = 0) override;

  // 检查是否与其它Tensor共享内存
  // 参数：b - DenseTensor对象
  // 返回：bool类型变量
  bool IsSharedWith(const DenseTensor& b) const;

  // 修改Tensor的Dims信息并分配内存
  // 参数：dims - DDim对象
  // 返回：None
  void ResizeAndAllocate(const DDim& dims);

  // 修改Tensor的Dims信息
  // 参数：dims - DDim对象
  // 返回：DenseTensor对象
  DenseTensor& Resize(const DDim& dims);

  // 重置Tensor的LoD信息
  // 参数：lod - LoD对象
  // 返回：None
  void ResetLoD(const LoD& lod);

  // 获取Tensor的内存大小
  // 参数：None
  // 返回：size_t类型变量
  size_t capacity() const;

  // 获取Tensor的不可修改数据指针
  // 模板参数：T - 数据类型
  // 参数：None
  // 返回：不可修改的T类型数据指针
  template <typename T>
  const T* data() const;

  // 获取Tensor的不可修改数据指针
  // 参数：None
  // 返回：不可修改的void类型数据指针
  const void* data() const;

  // 获取Tensor的可修改内存数据指针
  // 模板参数：T - 数据类型
  // 参数：None
  // 返回：可修改的T类型数据指针
  template <typename T>
  T* data();

  // 获取Tensor的可修改内存数据指针
  // 参数：None
  // 返回：可修改的void类型数据指针
  void* data();
```

## 其它Tensor类型

- `TensorBase`：请参照[tensor_base.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/tensor_base.h)
- `SelectedRows`：请参照[selected_rows.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/selected_rows.h)
- `SparseCooTensor`：请参照[sparse_coo_tensor.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/sparse_coo_tensor.h)
- `SparseCsrTensor`：请参照[sparse_csr_tensor.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/sparse_csr_tensor.h)


## 相关内容

- `Allocation`与`Allocator`：请参照[allocator.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/allocator.h)
- `DenseTensorMeta`：请参照[tensor_meta.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/tensor_meta.h)
- `DDim`：请参照[ddim.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/ddim.h)
- `LoD`：请参照[lod_utils.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/core/lod_utils.h)
- `DataType`：请参照[data_type.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/common/data_type.h)
- `DataLayout`：请参照[layout.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/common/layout.h)
- `Place`：请参照[place.h](https://github.com/PaddlePaddle/Paddle/blob/develop/paddle/phi/common/place.h)
