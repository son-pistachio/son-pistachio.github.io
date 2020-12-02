---
layout: post
title: "NVIDIA CUDA 기초 튜토리얼 (1)"
date: 2018-11-08
categories: CUDA
tags: [tutorial, CUDA, accelerated computing, NVIDIA]
---



NVIDIA 사의 공식 튜토리얼 `Fundamentals of Accelerated Computing C/C++`을 학습하며 정리한 내용.



# Accelerating Applications with CUDA C/C++



## GPU-accelerated Vs. CPU-only Applications

<div align="center"><iframe src="https://docs.google.com/presentation/d/e/2PACX-1vTdUbQjoEYAtcPCMX4ZVLa9gE0rbO3ODClJqgtzRaXoFgVmTJrOkXGDAYmA0BsuTEaokTASv84JsKLm/embed?start=false&loop=false&delayms=3000" frameborder="0" width="700" height="430" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe></div>



## Writing Application Code for the GPU

```cpp
void CPUFunction()
{
  printf("This function is defined to run on the CPU.\n");
}

__global__ void GPUFunction()
{
  printf("This function is defined to run on the GPU.\n");
}

int main()
{
  CPUFunction();

  GPUFunction<<<1, 1>>>();
  cudaDeviceSynchronize();
}
```

- `__global__ void GPUFunction()`	
  - `__global__`이라는 키워드가 GPU에서 돌아간다는 사실을 명시해준다.



- `GPUFunction<<1,1>>();`

  - GPU에서 작동하는 이러한 함수를 **kernel**이라 부르며, **thread hierarchy**를 명시해준다고 한다.
  - 인자 중 앞의 `1`은 실행될 쓰레드 그룹의 개수를 명시하며, `block`이라고 부른다. 
  - 인자 중 뒤의 `1`은 각 `block` 내에 몇 개의 쓰레드가 실행될 것인지를 명시한다.

  좀 더 자세한 내용은 뒤에서..

- `cudaDeviceSynchronize();`

  - 이후 계산된 값을 CPU와 synchronize하여 작동하게 하기 위해서는, 이 함수를 사용하여야 한다.



## Compiling and Running Accelerated CUDA Code

`.c` 파일을 `gcc`로 컴파일하는 것처럼, `.cu` 파일은 `nvcc`라는 [**NVIDIA CUDA Compiler**](http://docs.nvidia.com/cuda/cuda-compiler-driver-nvcc/index.html)로 컴파일한다. 다음과 같이 쓸 수 있다.

`nvcc -arch=sm_70 -o out some-CUDA.cu -run`

옵션은 다음과 같다.

- `-arch`: 컴파일되는 환경의 GPU 아키텍쳐를 명시해준다. `sm_70`의 경우 Volta 아키텍쳐를 명시해준다.
- `-o`: 아웃풋 파일의 이름을 명시해준다.
- `-run`: 편의를 위한 옵션. 이 옵션을 쓰면 컴파일한 바이너리 파일을 실행해준다.



## CUDA Thread Hierarchy

<div align="center"><iframe src="https://docs.google.com/presentation/d/e/2PACX-1vQYti_rVyWNNOccK6Slxd1VqazuqO5IhP17tmk-yTZAQfPEVpF14aZF9Vo3XkrDbFetNLTm_Pnk7JvD/embed?start=false&loop=false&delayms=3000" frameborder="0" width="700" height="430" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe></div>

CUDA Thread Hierarchy에서의 용어 좀 더 자세히 설명

- `kernel`: GPU function을 부르는 용어. `kernel`은 `execution configuration`에 따라 실행된다.
- `thread`: GPU 작업의 기본 단위. 여러 `thread`가 병렬적으로 작동한다.
- `block`: `thread`의 모임을 `block`이라 한다.
- `grid`: 주어진 `kernel`의 `execution configuration`에서 `block`들의 모임, 그러니까 전체를 `grid`라 부른다.



## CUDA-Provided Thread Hierarchy Variables

<div align="center"><iframe src="https://docs.google.com/presentation/d/e/2PACX-1vSVS21bI-cje3Cqtxke-LHcvxk1ZxvZF-F35bgHSKfvNsvkGklCeqwlXHCDPJey5meZ1vTVYMqiF0UV/embed?start=false&loop=false&delayms=3000" frameborder="0" width="700" height="430" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe></div>

CUDA Thread Hierarchy에서는 미리 정해진 변수를 통해 각 `block`과 `thread`에 접근할 수 있다.

- `gridDim.x`: `grid` 내에 있는 `block`의 개수. performWork<<<2,4>>>()와 같은 `kernel`의 경우 2가 된다.
- `blockIdx.x`: `grid` 내에 있는 `block`들 중 해당 `block`의 위치 인덱스. performWork<<<2,4>>>()와 같은 `kernel`을 실행한다면 `0`,`1`이 될 수 있다.
- `blockDim.x`: `block` 내에 있는 `thread`의 개수. performWork<<<2,4>>>()와 같은 `kernel`의 경우 `4`가 된다. 한 `grid` 내에 있는 모든 `block`은 같은 수의 `thread`를 가진다.
- `threadIdx.x`: `block` 내에 있는 `thread` 중 해당 `thread`의 위치 인덱스. performWork<<<2,4>>>()와 같은 `kernel`의 경우 `0`,`1`,`2`,`3` 중 하나가 된다.

## Accelerating For Loops

반복문을 가속화하는 법.



### Exercise: Accelerating a For Loop with a Single Block of Threads

```cpp
#include <stdio.h>

/*
 * Refactor `loop` to be a CUDA Kernel. The new kernel should
 * only do the work of 1 iteration of the original loop.
 */

void loop(int N)
{
  for (int i = 0; i < N; ++i)
  {
    printf("This is iteration number %d\n", i);
  }
}

int main()
{
  /*
   * When refactoring `loop` to launch as a kernel, be sure
   * to use the execution configuration to control how many
   * "iterations" to perform.
   *
   * For this exercise, only use 1 block of threads.
   */

  int N = 10;
  loop(N);
}
```

이런 반복문을 어떻게 GPU로 가속할 수 있을까? 병렬화를 하기 위해서는 2가지 단계를 꼭 거쳐야 한다:

- `kernel`은 해당 반복문에서 딱 한 번의 반복 작업만 하도록 설계되어야 한다.
- `kernel`이 다른 `kernel`에 대해서 알지 못하기 때문에, `execution configuration`이 해당 반복문에서 반복되는 작업의 수에 맞춰 선언되어야 한다.

우리가 위에서 배운 Thread Hierarchy Variable을 활용하면 이를 달성할 수 있다.

**Solution)**

```c++
#include <stdio.h>

/*
 * Refactor `loop` to be a CUDA Kernel. The new kernel should
 * only do the work of 1 iteration of the original loop.
 */

__global__ void loop()
{
  
  printf("This is iteration number %d\n", threadIdx.x);
  
}

int main()
{
  /*
   * When refactoring `loop` to launch as a kernel, be sure
   * to use the execution configuration to control how many
   * "iterations" to perform.
   *
   * For this exercise, only use 1 block of threads.
   */

  int N = 10;
  loop<<<1,N>>>();
  cudaDeviceSynchronize();
}
```



## Coordinating Parallel Threads

<div align="center"><iframe src="https://docs.google.com/presentation/d/e/2PACX-1vSfi8LAinJ1RqTzlB2vRsAcDzCCk9gZov5rQODN5rtRMPt57UizCVv5LSVZ5WLxGtrsMm7FIkLb0wMR/embed?start=false&loop=false&delayms=3000" frameborder="0" width="700" height="430" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe></div>

각 `block`에 존재할 수 있는 `thread`의 개수는 최대 1024개로 한계가 있다. 따라서 병렬처리의 효과를 더 크게 누리기 위해서는 여러 `block`들 간의 coordinate를 잘 해야 한다.

GPU `thread`에 data를 할당하기 위해, 각 `thread`의 인덱스를 활용한 데이터 분배 접근 전략을 활용한다.

각 `block`의 사이즈는 `blockDim.x`로 알 수 있고, 인덱스는 `blockIdx.x`로 접근할 수 있다. 또 각 `thread`의 인덱스는 `threadIdx.x`로 접근할 수 있다.

따라서 `threadIdx.x` + `blockIdx.x` * `blockDim.x`라는 공식을 활용하여 데이터를 `thread`에 매핑할 수 있다.



### Exercise: Accelerating a For Loop with Multiple Blocks of Threads

아까 위에서 병렬화 한 반복문을, 이번에는 최소 2개 이상의 `block`을 활용하여 병렬화시켜보자.



## Allocating Memory to be accessed on the GPU and the CPU

CPU-only application에서는 C가 `malloc`과 `free`를 사용해 메모리를 할당하고 해제하지만, GPU 가속을 할 때는 대신 `cudaMallocManaged`와 `cudaFree`를 사용한다. 사용 예시는 다음과 같으니 비교해보자.

```cpp
// CPU-only

int N = 2<<20;
size_t size = N * sizeof(int);

int *a;
a = (int *)malloc(size);

// Use `a` in CPU-only program.

free(a);
// Accelerated

int N = 2<<20;
size_t size = N * sizeof(int);

int *a;
// Note the address of `a` is passed as first argument.
cudaMallocManaged(&a, size);

// Use `a` on the CPU and/or on any GPU in the accelerated system.

cudaFree(a);
```



### Exercise: Array Manipulation on both the Host and Device

```c++
#include <stdio.h>

/*
 * Initialize array values on the host.
 */

void init(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    a[i] = i;
  }
}

/*
 * Double elements in parallel on the GPU.
 */

__global__
void doubleElements(int *a, int N)
{
  int i;
  i = blockIdx.x * blockDim.x + threadIdx.x;
  if (i < N)
  {
    a[i] *= 2;
  }
}

/*
 * Check all elements have been doubled on the host.
 */

bool checkElementsAreDoubled(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    if (a[i] != i*2) return false;
  }
  return true;
}

int main()
{
  int N = 100;
  int *a;

  size_t size = N * sizeof(int);

  /*
   * Refactor this memory allocation to provide a pointer
   * `a` that can be used on both the host and the device.
   */

  a = (int *)malloc(size);

  init(a, N);

  size_t threads_per_block = 10;
  size_t number_of_blocks = 10;

  /*
   * This launch will not work until the pointer `a` is also
   * available to the device.
   */

  doubleElements<<<number_of_blocks, threads_per_block>>>(a, N);
  cudaDeviceSynchronize();

  bool areDoubled = checkElementsAreDoubled(a, N);
  printf("All elements were doubled? %s\n", areDoubled ? "TRUE" : "FALSE");

  /*
   * Refactor to free memory that has been allocated to be
   * accessed by both the host and the device.
   */

  free(a);
}
```

위 코드를 배열 포인터 `a`가 CPU와 GPU 코드에서 모두 쓰일 수 있게, 또 `a`를 정확히 메모리 해제해야 한다는 점에 유의해서 고쳐보자.



**Solution)**

```c++
#include <stdio.h>

void init(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    a[i] = i;
  }
}

__global__
void doubleElements(int *a, int N)
{
  int i;
  i = blockIdx.x * blockDim.x + threadIdx.x;
  if (i < N)
  {
    a[i] *= 2;
  }
}

bool checkElementsAreDoubled(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    if (a[i] != i*2) return false;
  }
  return true;
}

int main()
{
  int N = 100;
  int *a;

  size_t size = N * sizeof(int);

  cudaMallocManaged(&a, size);
  init(a, N);

  size_t threads_per_block = 10;
  size_t number_of_blocks = 10;


  doubleElements<<<number_of_blocks, threads_per_block>>>(a, N);
  cudaDeviceSynchronize();

  bool areDoubled = checkElementsAreDoubled(a, N);
  printf("All elements were doubled? %s\n", areDoubled ? "TRUE" : "FALSE");

  cudaFree(a);
}
```



## Grid Size Work Amount Mismatch

우리가 사용하려는 데이터가 `grid` 사이즈에 딱 맞으면 상관 없지만, 만약 그것보다 부족한 경우 사이즈가 맞지 않는다는 문제가 발생한다. e.g.) `grid` 내에 `thread` 개수가 8개인데 사용할 데이터는 5개밖에 없으면  `threadIdx.x` + `blockIdx.x` * `blockDim.x` 공식으로 할당할 때 5,6,7번은 문제가 생긴다.

```cpp
__global__ some_kernel(int N)
{
  int idx = threadIdx.x + blockIdx.x * blockDim.x;

  if (idx < N) // Check to make sure `idx` maps to some value within `N`
  {
    // Only do work if it does
  }
}

// Assume `N` is known
int N = 100000;

// Assume we have a desire to set `threads_per_block` exactly to `256`
size_t threads_per_block = 256;

// Ensure there are at least `N` threads in the grid, but only 1 block's worth extra
size_t number_of_blocks = (N + threads_per_block - 1) / threads_per_block;

some_kernel<<<number_of_blocks, threads_per_block>>>(N);
```

그래서 위와 같이 `some_kernel` 함수의 `if`문처럼 인덱스가 데이터의 크기보다 작을 때만 특정 기능을 실행하도록 설정해주면 된다.



### Exercise: Accelerating a For Loop with a Mismatched Execution Configuration

```c++
#include <stdio.h>

/*
 * Currently, `initializeElementsTo`, if executed in a thread whose
 * `i` is calculated to be greater than `N`, will try to access a value
 * outside the range of `a`.
 *
 * Refactor the kernel defintition to prevent our of range accesses.
 */

__global__ void initializeElementsTo(int initialValue, int *a, int N)
{
  int i = threadIdx.x + blockIdx.x * blockDim.x;
  a[i] = initialValue;
}

int main()
{
  /*
   * Do not modify `N`.
   */

  int N = 1000;

  int *a;
  size_t size = N * sizeof(int);

  cudaMallocManaged(&a, size);

  /*
   * Assume we have reason to want the number of threads
   * fixed at `256`: do not modify `threads_per_block`.
   */

  size_t threads_per_block = 256;

  /*
   * Assign a value to `number_of_blocks` that will
   * allow for a working execution configuration given
   * the fixed values for `N` and `threads_per_block`.
   */

  size_t number_of_blocks = 0;

  int initialValue = 6;

  initializeElementsTo<<<number_of_blocks, threads_per_block>>>(initialValue, a, N);
  cudaDeviceSynchronize();

  /*
   * Check to make sure all values in `a`, were initialized.
   */

  for (int i = 0; i < N; ++i)
  {
    if(a[i] != initialValue)
    {
      printf("FAILURE: target value: %d\t a[%d]: %d\n", initialValue, i, a[i]);
      exit(1);
    }
  }
  printf("SUCCESS!\n");

  cudaFree(a);
}
```

이 코드는 1000개의 integer를 `cudaMallocManaged`를 활용해 메모리 할당하고 있고, `thread_per_blocks` 라는 이름의 변수로 `block` 당 최대 `thread` 개수를 정의하고 있다. 이에 따라 `number_of_blocks` 변수에 필요한 `block`의 개수를 구해 할당해주고, `initializeElementsTo` 함수에다 데이터 수보다 인덱스가 넘치는 경우 예외를 처리해주는 내용의 코드를 추가해주자.



**Solution)**

```c++
#include <stdio.h>

/*
 * Currently, `initializeElementsTo`, if executed in a thread whose
 * `i` is calculated to be greater than `N`, will try to access a value
 * outside the range of `a`.
 *
 * Refactor the kernel defintition to prevent our of range accesses.
 */

__global__ void initializeElementsTo(int initialValue, int *a, int N)
{
  int i = threadIdx.x + blockIdx.x * blockDim.x;
  if(i < N){
      a[i] = initialValue;
  }
}

int main()
{
  /*
   * Do not modify `N`.
   */

  int N = 1000;

  int *a;
  size_t size = N * sizeof(int);

  cudaMallocManaged(&a, size);

  /*
   * Assume we have reason to want the number of threads
   * fixed at `256`: do not modify `threads_per_block`.
   */

  size_t threads_per_block = 256;

  /*
   * Assign a value to `number_of_blocks` that will
   * allow for a working execution configuration given
   * the fixed values for `N` and `threads_per_block`.
   */

  size_t number_of_blocks = (N + threads_per_block - 1) / threads_per_block;

  int initialValue = 6;

  initializeElementsTo<<<number_of_blocks, threads_per_block>>>(initialValue, a, N);
  cudaDeviceSynchronize();

  /*
   * Check to make sure all values in `a`, were initialized.
   */

  for (int i = 0; i < N; ++i)
  {
    if(a[i] != initialValue)
    {
      printf("FAILURE: target value: %d\t a[%d]: %d\n", initialValue, i, a[i]);
      exit(1);
    }
  }
  printf("SUCCESS!\n");

  cudaFree(a);
}
```



## Grid-Stride Loops

<div align="center"><iframe src="https://docs.google.com/presentation/d/e/2PACX-1vTSfcPagyv7ObRnhygFnKrvDIDa-wUuc3yR-qs7xd4gQxProMOqXzNqe8y9vz711cLIbPp1qYJc7R3l/embed?start=false&loop=false&delayms=3000" frameborder="0" width="700" height="430" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe></div>

이번에는 데이터의 개수가 `grid` 내에 존재하는 `thread`의 개수보다 많은 경우이다. 이 경우 남은 데이터들을 처리해줄 수 없기 때문에 `grid-stride loops`라는 방법을 사용한다. 

이 방법은 `kernel` 내에서 반복문을 사용하는 것인데, 매 반복마다 `grid` 내의 `thread`의 개수인 `gridDim.x` \* `blockDim.x`만큼 인덱스에 더해주면서 모든 데이터를 처리하는 방법이다.



### Exercise: Use a Grid-Stride Loop to Manipulate an Array Larger than the Grid

```c++
#include <stdio.h>

void init(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    a[i] = i;
  }
}

/*
 * In the current application, `N` is larger than the grid.
 * Refactor this kernel to use a grid-stride loop in order that
 * each parallel thread work on more than one element of the array.
 */

__global__
void doubleElements(int *a, int N)
{
  int i;
  i = blockIdx.x * blockDim.x + threadIdx.x;
  if (i < N)
  {
    a[i] *= 2;
  }
}

bool checkElementsAreDoubled(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    if (a[i] != i*2) return false;
  }
  return true;
}

int main()
{
  /*
   * `N` is greater than the size of the grid (see below).
   */

  int N = 10000;
  int *a;

  size_t size = N * sizeof(int);
  cudaMallocManaged(&a, size);

  init(a, N);

  /*
   * The size of this grid is 256*32 = 8192.
   */

  size_t threads_per_block = 256;
  size_t number_of_blocks = 32;

  doubleElements<<<number_of_blocks, threads_per_block>>>(a, N);
  cudaDeviceSynchronize();

  bool areDoubled = checkElementsAreDoubled(a, N);
  printf("All elements were doubled? %s\n", areDoubled ? "TRUE" : "FALSE");

  cudaFree(a);
}
```

`grid` 내의 `thread` 개수는 256*32 = 8192개지만 전체 데이터는 10000개로 이를 초과한다. `doubleElements` 커널을 `grid-stride loops` 방식으로 수정하여 모든 데이터를 연산할 수 있게 해보자.

**Solution)**

```c++
#include <stdio.h>

void init(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    a[i] = i;
  }
}

__global__
void doubleElements(int *a, int N)
{

  /*
   * Use a grid-stride loop so each thread does work
   * on more than one element in the array.
   */

  int idx = blockIdx.x * blockDim.x + threadIdx.x;
  int stride = gridDim.x * blockDim.x;

  for (int i = idx; i < N; i += stride)
  {
    a[i] *= 2;
  }
}

bool checkElementsAreDoubled(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    if (a[i] != i*2) return false;
  }
  return true;
}

int main()
{
  int N = 10000;
  int *a;

  size_t size = N * sizeof(int);
  cudaMallocManaged(&a, size);

  init(a, N);

  size_t threads_per_block = 256;
  size_t number_of_blocks = 32;

  doubleElements<<<number_of_blocks, threads_per_block>>>(a, N);
  cudaDeviceSynchronize();

  bool areDoubled = checkElementsAreDoubled(a, N);
  printf("All elements were doubled? %s\n", areDoubled ? "TRUE" : "FALSE");

  cudaFree(a);
}
```



## Error Handling

어느 어플리케이션에서나 `Error Handling`은 중요하다. 대부분의 CUDA 함수는 `cudaError_t`라는 자료형으로 결과값을 반환하는데, 이걸 확인하면 에러가 발생했는지 아닌지를 확인할 수 있다. `cudaMallocManaged` 함수의 경우 다음과 같다.

```cpp
cudaError_t err;
err = cudaMallocManaged(&a, N)                    // Assume the existence of `a` and `N`.

if (err != cudaSuccess)                           // `cudaSuccess` is provided by CUDA.
{
  printf("Error: %s\n", cudaGetErrorString(err)); // `cudaGetErrorString` is provided by CUDA.
}
```

그런데 `kernel` 함수는 void 함수로 짜서 `cudaError_t`를 반환하지 않는데, 이 경우 CUDA는 `cudaGetLastError` 함수를 제공한다. 

```cpp
/*
 * This launch should cause an error, but the kernel itself
 * cannot return it.
 */

someKernel<<<1, -1>>>();  // -1 is not a valid number of threads.

cudaError_t err;
err = cudaGetLastError(); // `cudaGetLastError` will return the error from above.
if (err != cudaSuccess)
{
  printf("Error: %s\n", cudaGetErrorString(err));
}
```

만약 asynchronous하게 발생하는 에러를 잡고 싶다면(예를 들면 asynchronous하게 작동하는 `kernel`) synchronize하게 하는 CUDA runtime API call (예를 들면 `cudaDeviceSynchronize`)을 잘 체크해서 잡아내야 한다.



### Exercise: Add Error Handling

```c++
#include <stdio.h>

void init(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    a[i] = i;
  }
}

__global__
void doubleElements(int *a, int N)
{

  int idx = blockIdx.x * blockDim.x + threadIdx.x;
  int stride = gridDim.x * blockDim.x;

  for (int i = idx; i < N + stride; i += stride)
  {
    a[i] *= 2;
  }
}

bool checkElementsAreDoubled(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    if (a[i] != i*2) return false;
  }
  return true;
}

int main()
{
  /*
   * Add error handling to this source code to learn what errors
   * exist, and then correct them. Googling error messages may be
   * of service if actions for resolving them are not clear to you.
   */

  int N = 10000;
  int *a;

  size_t size = N * sizeof(int);
  cudaMallocManaged(&a, size);

  init(a, N);

  size_t threads_per_block = 2048;
  size_t number_of_blocks = 32;

  doubleElements<<<number_of_blocks, threads_per_block>>>(a, N);
  cudaDeviceSynchronize();

  bool areDoubled = checkElementsAreDoubled(a, N);
  printf("All elements were doubled? %s\n", areDoubled ? "TRUE" : "FALSE");

  cudaFree(a);
}
```



**Solution)**

```c++
#include <stdio.h>

void init(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    a[i] = i;
  }
}

__global__
void doubleElements(int *a, int N)
{

  int idx = blockIdx.x * blockDim.x + threadIdx.x;
  int stride = gridDim.x * blockDim.x;

  /*
   * The previous code (now commented out) attempted
   * to access an element outside the range of `a`.
   */

  // for (int i = idx; i < N + stride; i += stride)
  for (int i = idx; i < N; i += stride)
  {
    a[i] *= 2;
  }
}

bool checkElementsAreDoubled(int *a, int N)
{
  int i;
  for (i = 0; i < N; ++i)
  {
    if (a[i] != i*2) return false;
  }
  return true;
}

int main()
{
  int N = 10000;
  int *a;

  size_t size = N * sizeof(int);
  cudaMallocManaged(&a, size);

  init(a, N);

  /*
   * The previous code (now commented out) attempted to launch
   * the kernel with more than the maximum number of threads per
   * block, which is 1024.
   */

  size_t threads_per_block = 1024;
  /* size_t threads_per_block = 2048; */
  size_t number_of_blocks = 32;

  cudaError_t syncErr, asyncErr;

  doubleElements<<<number_of_blocks, threads_per_block>>>(a, N);

  /*
   * Catch errors for both the kernel launch above and any
   * errors that occur during the asynchronous `doubleElements`
   * kernel execution.
   */

  syncErr = cudaGetLastError();
  asyncErr = cudaDeviceSynchronize();

  /*
   * Print errors should they exist.
   */

  if (syncErr != cudaSuccess) printf("Error: %s\n", cudaGetErrorString(syncErr));
  if (asyncErr != cudaSuccess) printf("Error: %s\n", cudaGetErrorString(asyncErr));

  bool areDoubled = checkElementsAreDoubled(a, N);
  printf("All elements were doubled? %s\n", areDoubled ? "TRUE" : "FALSE");

  cudaFree(a);
}
```



## CUDA Error Handling Function

다음은 에러를 확인하기 편하게 해주는 매크로 함수다. 다른 exercise를 풀 때 편하게 사용하면 된다.

```cpp
#include <stdio.h>
#include <assert.h>

inline cudaError_t checkCuda(cudaError_t result)
{
  if (result != cudaSuccess) {
    fprintf(stderr, "CUDA Runtime Error: %s\n", cudaGetErrorString(result));
    assert(result == cudaSuccess);
  }
  return result;
}

int main()
{

/*
 * The macro can be wrapped around any function returning
 * a value of type `cudaError_t`.
 */

  checkCuda( cudaDeviceSynchronize() )
}
```



## 요약

Accelerating Applications with CUDA C/C++에서 배운 내용 요약.

- Write, compile, and run C/C++ programs that both call CPU functions and **launch** GPU **kernels**.
- Control parallel **thread hierarchy** using **execution configuration**.
- Refactor serial loops to execute their iterations in parallel on a GPU.
- Allocate and free memory available to both CPUs and GPUs.
- Handle errors generated by CUDA code.



### Final Exercise: Accelerate Vector Addition Application

```c++
#include <stdio.h>

void initWith(float num, float *a, int N)
{
  for(int i = 0; i < N; ++i)
  {
    a[i] = num;
  }
}

void addVectorsInto(float *result, float *a, float *b, int N)
{
  for(int i = 0; i < N; ++i)
  {
    result[i] = a[i] + b[i];
  }
}

void checkElementsAre(float target, float *array, int N)
{
  for(int i = 0; i < N; i++)
  {
    if(array[i] != target)
    {
      printf("FAIL: array[%d] - %0.0f does not equal %0.0f\n", i, array[i], target);
      exit(1);
    }
  }
  printf("SUCCESS! All values added correctly.\n");
}

int main()
{
  const int N = 2<<20;
  size_t size = N * sizeof(float);

  float *a;
  float *b;
  float *c;

  a = (float *)malloc(size);
  b = (float *)malloc(size);
  c = (float *)malloc(size);

  initWith(3, a, N);
  initWith(4, b, N);
  initWith(0, c, N);

  addVectorsInto(c, a, b, N);

  checkElementsAre(7, c, N);

  free(a);
  free(b);
  free(c);
}
```

위 코드는 CPU로만 작동하는 벡터 더하기 어플리케이션이다. 여기 있는 `addVectorsInto` 함수를 CUDA `kernel`로 만들어 GPU 병렬 연산을 할 수 있게 만들어보자. 다음을 유의해서 코드를 짜보자.

- `addVectorsInto`를 CUDA `kernel`로 만들기
- `addVectorsInto`가 CUDA `kernel`로 작동하는 적절한 execution configuration을 찾고 실행하기
- 메모리 할당과 해제를 적절히 해서 `a`, `b`, `result` 벡터가 CPU/GPU에서 모두 접근 가능하도록 하기
- `addVectorsInto`를 리팩토링하자: it will be launched inside of a single thread, and only needs to do one thread's worth of work on the input vectors. Be certain the thread will never try to access elements outside the range of the input vectors, and take care to note whether or not the thread needs to do work on more than one element of the input vectors.
- CUDA 코드가 잘못될 수 있는 부분에 적절히 error handling을 하자.



**Solution)**

```c++
#include <stdio.h>
#include <assert.h>

inline cudaError_t checkCuda(cudaError_t result)
{
  if (result != cudaSuccess) {
    fprintf(stderr, "CUDA Runtime Error: %s\n", cudaGetErrorString(result));
    assert(result == cudaSuccess);
  }
  return result;
}

void initWith(float num, float *a, int N)
{
  for(int i = 0; i < N; ++i)
  {
    a[i] = num;
  }
}

__global__
void addVectorsInto(float *result, float *a, float *b, int N)
{
  int index = threadIdx.x + blockIdx.x * blockDim.x;
  int stride = blockDim.x * gridDim.x;

  for(int i = index; i < N; i += stride)
  {
    result[i] = a[i] + b[i];
  }
}

void checkElementsAre(float target, float *array, int N)
{
  for(int i = 0; i < N; i++)
  {
    if(array[i] != target)
    {
      printf("FAIL: array[%d] - %0.0f does not equal %0.0f\n", i, array[i], target);
      exit(1);
    }
  }
  printf("SUCCESS! All values added correctly.\n");
}

int main()
{
  const int N = 2<<20;
  size_t size = N * sizeof(float);

  float *a;
  float *b;
  float *c;

  checkCuda( cudaMallocManaged(&a, size) );
  checkCuda( cudaMallocManaged(&b, size) );
  checkCuda( cudaMallocManaged(&c, size) );

  initWith(3, a, N);
  initWith(4, b, N);
  initWith(0, c, N);

  size_t threadsPerBlock;
  size_t numberOfBlocks;

  threadsPerBlock = 256;
  numberOfBlocks = (N + threadsPerBlock - 1) / threadsPerBlock;

  addVectorsInto<<<numberOfBlocks, threadsPerBlock>>>(c, a, b, N);

  checkCuda( cudaGetLastError() );
  checkCuda( cudaDeviceSynchronize() );

  checkElementsAre(7, c, N);

  checkCuda( cudaFree(a) );
  checkCuda( cudaFree(b) );
  checkCuda( cudaFree(c) );
}
```



## Advanced Content

The following exercises provide additional challenge for those with time and interest. They require the use of more advanced techniques, and provide less scaffolding. They are difficult and excellent for your development.

### Grids and Blocks of 2 and 3 Dimensions

Grids and blocks can be defined to have up to 3 dimensions. Defining them with multiple dimensions does not impact their performance in any way, but can be very helpful when dealing with data that has multiple dimensions, for example, 2d matrices. To define either grids or blocks with two or 3 dimensions, use CUDA's `dim3` type as such:

```cpp
dim3 threads_per_block(16, 16, 1);
dim3 number_of_blocks(16, 16, 1);
someKernel<<<number_of_blocks, threads_per_block>>>();
```

Given the example just above, the variables `gridDim.x`, `gridDim.y`, `blockDim.x`, and `blockDim.y` inside of `someKernel`, would all be equal to `16`.



### Exercise: Accelerate 2D Matrix Multiply Application

The file [`01-matrix-multiply-2d.cu`](http://ec2-18-191-146-97.us-east-2.compute.amazonaws.com/oiOcuiDi/edit/tasks/task1/task/01_AC_CUDA_C/08-matrix-multiply/01-matrix-multiply-2d.cu) contains a host function `matrixMulCPU` which is fully functional. Your task is to build out the `matrixMulGPU`CUDA kernel. The source code will execute the matrix multiplication with both functions, and compare their answers to verify the correctness of the CUDA kernel you will be writing. Use the following guidelines to support your work and refer to [the solution](http://ec2-18-191-146-97.us-east-2.compute.amazonaws.com/oiOcuiDi/edit/tasks/task1/task/01_AC_CUDA_C/08-matrix-multiply/solutions/01-matrix-multiply-2d-solution.cu) if you get stuck:

- You will need to create an execution configuration whose arguments are both `dim3` values with the `x` and `y` dimensions set to greater than `1`.
- Inside the body of the kernel, you will need to establish the running thread's unique index within the grid per usual, but you should establish two indices for the thread: one for the x axis of the grid, and one for the y axis of the grid.

```c++
#include <stdio.h>

#define N  64

__global__ void matrixMulGPU( int * a, int * b, int * c )
{
  /*
   * Build out this kernel.
   */
}

/*
 * This CPU function already works, and will run to create a solution matrix
 * against which to verify your work building out the matrixMulGPU kernel.
 */

void matrixMulCPU( int * a, int * b, int * c )
{
  int val = 0;

  for( int row = 0; row < N; ++row )
    for( int col = 0; col < N; ++col )
    {
      val = 0;
      for ( int k = 0; k < N; ++k )
        val += a[row * N + k] * b[k * N + col];
      c[row * N + col] = val;
    }
}

int main()
{
  int *a, *b, *c_cpu, *c_gpu; // Allocate a solution matrix for both the CPU and the GPU operations

  int size = N * N * sizeof (int); // Number of bytes of an N x N matrix

  // Allocate memory
  cudaMallocManaged (&a, size);
  cudaMallocManaged (&b, size);
  cudaMallocManaged (&c_cpu, size);
  cudaMallocManaged (&c_gpu, size);

  // Initialize memory; create 2D matrices
  for( int row = 0; row < N; ++row )
    for( int col = 0; col < N; ++col )
    {
      a[row*N + col] = row;
      b[row*N + col] = col+2;
      c_cpu[row*N + col] = 0;
      c_gpu[row*N + col] = 0;
    }

  /*
   * Assign `threads_per_block` and `number_of_blocks` 2D values
   * that can be used in matrixMulGPU above.
   */

  dim3 threads_per_block;
  dim3 number_of_blocks;

  matrixMulGPU <<< number_of_blocks, threads_per_block >>> ( a, b, c_gpu );

  cudaDeviceSynchronize();

  // Call the CPU version to check our work
  matrixMulCPU( a, b, c_cpu );

  // Compare the two answers to make sure they are equal
  bool error = false;
  for( int row = 0; row < N && !error; ++row )
    for( int col = 0; col < N && !error; ++col )
      if (c_cpu[row * N + col] != c_gpu[row * N + col])
      {
        printf("FOUND ERROR at c[%d][%d]\n", row, col);
        error = true;
        break;
      }
  if (!error)
    printf("Success!\n");

  // Free all our allocated memory
  cudaFree(a); cudaFree(b);
  cudaFree( c_cpu ); cudaFree( c_gpu );
}
```



**Solution)**

```c++
#include <stdio.h>

#define N  64

__global__ void matrixMulGPU( int * a, int * b, int * c )
{
  int val = 0;

  int row = blockIdx.x * blockDim.x + threadIdx.x;
  int col = blockIdx.y * blockDim.y + threadIdx.y;

  if (row < N && col < N)
  {
    for ( int k = 0; k < N; ++k )
      val += a[row * N + k] * b[k * N + col];
    c[row * N + col] = val;
  }
}

void matrixMulCPU( int * a, int * b, int * c )
{
  int val = 0;

  for( int row = 0; row < N; ++row )
    for( int col = 0; col < N; ++col )
    {
      val = 0;
      for ( int k = 0; k < N; ++k )
        val += a[row * N + k] * b[k * N + col];
      c[row * N + col] = val;
    }
}

int main()
{
  int *a, *b, *c_cpu, *c_gpu;

  int size = N * N * sizeof (int); // Number of bytes of an N x N matrix

  // Allocate memory
  cudaMallocManaged (&a, size);
  cudaMallocManaged (&b, size);
  cudaMallocManaged (&c_cpu, size);
  cudaMallocManaged (&c_gpu, size);

  // Initialize memory
  for( int row = 0; row < N; ++row )
    for( int col = 0; col < N; ++col )
    {
      a[row*N + col] = row;
      b[row*N + col] = col+2;
      c_cpu[row*N + col] = 0;
      c_gpu[row*N + col] = 0;
    }

  dim3 threads_per_block (16, 16, 1); // A 16 x 16 block threads
  dim3 number_of_blocks ((N / threads_per_block.x) + 1, (N / threads_per_block.y) + 1, 1);

  matrixMulGPU <<< number_of_blocks, threads_per_block >>> ( a, b, c_gpu );

  cudaDeviceSynchronize(); // Wait for the GPU to finish before proceeding

  // Call the CPU version to check our work
  matrixMulCPU( a, b, c_cpu );

  // Compare the two answers to make sure they are equal
  bool error = false;
  for( int row = 0; row < N && !error; ++row )
    for( int col = 0; col < N && !error; ++col )
      if (c_cpu[row * N + col] != c_gpu[row * N + col])
      {
        printf("FOUND ERROR at c[%d][%d]\n", row, col);
        error = true;
        break;
      }
  if (!error)
    printf("Success!\n");

  // Free all our allocated memory
  cudaFree(a); cudaFree(b);
  cudaFree( c_cpu ); cudaFree( c_gpu );
}
```



### Exercise: Accelerate A Thermal Conductivity Application

In the following exercise, you will be accelerating an application that simulates the thermal conduction of silver in 2 dimensional space.

Convert the `step_kernel_mod` function inside [`01-heat-conduction.cu`](http://ec2-18-191-146-97.us-east-2.compute.amazonaws.com/oiOcuiDi/edit/tasks/task1/task/01_AC_CUDA_C/09-heat/01-heat-conduction.cu) to execute on the GPU, and modify the `main` function to properly allocate data for use on CPU and GPU. The `step_kernel_ref` function executes on the CPU and is used for error checking. Because this code involves floating point calculations, different processors, or even simply reording operations on the same processor, can result in slightly different results. For this reason the error checking code uses an error threshold, instead of looking for an exact match. Refer to [the solution](http://ec2-18-191-146-97.us-east-2.compute.amazonaws.com/oiOcuiDi/edit/tasks/task1/task/01_AC_CUDA_C/09-heat/solutions/01-heat-conduction-solution.cu) if you get stuck.

```c++
#include <stdio.h>
#include <math.h>

// Simple define to index into a 1D array from 2D space
#define I2D(num, c, r) ((r)*(num)+(c))

/*
 * `step_kernel_mod` is currently a direct copy of the CPU reference solution
 * `step_kernel_ref` below. Accelerate it to run as a CUDA kernel.
 */

void step_kernel_mod(int ni, int nj, float fact, float* temp_in, float* temp_out)
{
  int i00, im10, ip10, i0m1, i0p1;
  float d2tdx2, d2tdy2;


  // loop over all points in domain (except boundary)
  for ( int j=1; j < nj-1; j++ ) {
    for ( int i=1; i < ni-1; i++ ) {
      // find indices into linear memory
      // for central point and neighbours
      i00 = I2D(ni, i, j);
      im10 = I2D(ni, i-1, j);
      ip10 = I2D(ni, i+1, j);
      i0m1 = I2D(ni, i, j-1);
      i0p1 = I2D(ni, i, j+1);

      // evaluate derivatives
      d2tdx2 = temp_in[im10]-2*temp_in[i00]+temp_in[ip10];
      d2tdy2 = temp_in[i0m1]-2*temp_in[i00]+temp_in[i0p1];

      // update temperatures
      temp_out[i00] = temp_in[i00]+fact*(d2tdx2 + d2tdy2);
    }
  }
}

void step_kernel_ref(int ni, int nj, float fact, float* temp_in, float* temp_out)
{
  int i00, im10, ip10, i0m1, i0p1;
  float d2tdx2, d2tdy2;


  // loop over all points in domain (except boundary)
  for ( int j=1; j < nj-1; j++ ) {
    for ( int i=1; i < ni-1; i++ ) {
      // find indices into linear memory
      // for central point and neighbours
      i00 = I2D(ni, i, j);
      im10 = I2D(ni, i-1, j);
      ip10 = I2D(ni, i+1, j);
      i0m1 = I2D(ni, i, j-1);
      i0p1 = I2D(ni, i, j+1);

      // evaluate derivatives
      d2tdx2 = temp_in[im10]-2*temp_in[i00]+temp_in[ip10];
      d2tdy2 = temp_in[i0m1]-2*temp_in[i00]+temp_in[i0p1];

      // update temperatures
      temp_out[i00] = temp_in[i00]+fact*(d2tdx2 + d2tdy2);
    }
  }
}

int main()
{
  int istep;
  int nstep = 200; // number of time steps

  // Specify our 2D dimensions
  const int ni = 200;
  const int nj = 100;
  float tfac = 8.418e-5; // thermal diffusivity of silver

  float *temp1_ref, *temp2_ref, *temp1, *temp2, *temp_tmp;

  const int size = ni * nj * sizeof(float);

  temp1_ref = (float*)malloc(size);
  temp2_ref = (float*)malloc(size);
  temp1 = (float*)malloc(size);
  temp2 = (float*)malloc(size);

  // Initialize with random data
  for( int i = 0; i < ni*nj; ++i) {
    temp1_ref[i] = temp2_ref[i] = temp1[i] = temp2[i] = (float)rand()/(float)(RAND_MAX/100.0f);
  }

  // Execute the CPU-only reference version
  for (istep=0; istep < nstep; istep++) {
    step_kernel_ref(ni, nj, tfac, temp1_ref, temp2_ref);

    // swap the temperature pointers
    temp_tmp = temp1_ref;
    temp1_ref = temp2_ref;
    temp2_ref= temp_tmp;
  }

  // Execute the modified version using same data
  for (istep=0; istep < nstep; istep++) {
    step_kernel_mod(ni, nj, tfac, temp1, temp2);

    // swap the temperature pointers
    temp_tmp = temp1;
    temp1 = temp2;
    temp2= temp_tmp;
  }

  float maxError = 0;
  // Output should always be stored in the temp1 and temp1_ref at this point
  for( int i = 0; i < ni*nj; ++i ) {
    if (abs(temp1[i]-temp1_ref[i]) > maxError) { maxError = abs(temp1[i]-temp1_ref[i]); }
  }

  // Check and see if our maxError is greater than an error bound
  if (maxError > 0.0005f)
    printf("Problem! The Max Error of %.5f is NOT within acceptable bounds.\n", maxError);
  else
    printf("The Max Error of %.5f is within acceptable bounds.\n", maxError);

  free( temp1_ref );
  free( temp2_ref );
  free( temp1 );
  free( temp2 );

  return 0;
}
```



**Solution)**

```c++
#include <stdio.h>
#include <math.h>

// Simple define to index into a 1D array from 2D space
#define I2D(num, c, r) ((r)*(num)+(c))

__global__
void step_kernel_mod(int ni, int nj, float fact, float* temp_in, float* temp_out)
{
  int i00, im10, ip10, i0m1, i0p1;
  float d2tdx2, d2tdy2;

  int j = blockIdx.x * blockDim.x + threadIdx.x;
  int i = blockIdx.y * blockDim.y + threadIdx.y;

  // loop over all points in domain (except boundary)
  if (j > 0 && i > 0 && j < nj-1 && i < ni-1) {
    // find indices into linear memory
    // for central point and neighbours
    i00 = I2D(ni, i, j);
    im10 = I2D(ni, i-1, j);
    ip10 = I2D(ni, i+1, j);
    i0m1 = I2D(ni, i, j-1);
    i0p1 = I2D(ni, i, j+1);

    // evaluate derivatives
    d2tdx2 = temp_in[im10]-2*temp_in[i00]+temp_in[ip10];
    d2tdy2 = temp_in[i0m1]-2*temp_in[i00]+temp_in[i0p1];

    // update temperatures
    temp_out[i00] = temp_in[i00]+fact*(d2tdx2 + d2tdy2);
  }
}

void step_kernel_ref(int ni, int nj, float fact, float* temp_in, float* temp_out)
{
  int i00, im10, ip10, i0m1, i0p1;
  float d2tdx2, d2tdy2;


  // loop over all points in domain (except boundary)
  for ( int j=1; j < nj-1; j++ ) {
    for ( int i=1; i < ni-1; i++ ) {
      // find indices into linear memory
      // for central point and neighbours
      i00 = I2D(ni, i, j);
      im10 = I2D(ni, i-1, j);
      ip10 = I2D(ni, i+1, j);
      i0m1 = I2D(ni, i, j-1);
      i0p1 = I2D(ni, i, j+1);

      // evaluate derivatives
      d2tdx2 = temp_in[im10]-2*temp_in[i00]+temp_in[ip10];
      d2tdy2 = temp_in[i0m1]-2*temp_in[i00]+temp_in[i0p1];

      // update temperatures
      temp_out[i00] = temp_in[i00]+fact*(d2tdx2 + d2tdy2);
    }
  }
}

int main()
{
  int istep;
  int nstep = 200; // number of time steps

  // Specify our 2D dimensions
  const int ni = 200;
  const int nj = 100;
  float tfac = 8.418e-5; // thermal diffusivity of silver

  float *temp1_ref, *temp2_ref, *temp1, *temp2, *temp_tmp;

  const int size = ni * nj * sizeof(float);

  temp1_ref = (float*)malloc(size);
  temp2_ref = (float*)malloc(size);
  cudaMallocManaged(&temp1, size);
  cudaMallocManaged(&temp2, size);

  // Initialize with random data
  for( int i = 0; i < ni*nj; ++i) {
    temp1_ref[i] = temp2_ref[i] = temp1[i] = temp2[i] = (float)rand()/(float)(RAND_MAX/100.0f);
  }

  // Execute the CPU-only reference version
  for (istep=0; istep < nstep; istep++) {
    step_kernel_ref(ni, nj, tfac, temp1_ref, temp2_ref);

    // swap the temperature pointers
    temp_tmp = temp1_ref;
    temp1_ref = temp2_ref;
    temp2_ref= temp_tmp;
  }

  dim3 tblocks(32, 16, 1);
  dim3 grid((nj/tblocks.x)+1, (ni/tblocks.y)+1, 1);
  cudaError_t ierrSync, ierrAsync;

  // Execute the modified version using same data
  for (istep=0; istep < nstep; istep++) {
    step_kernel_mod<<< grid, tblocks >>>(ni, nj, tfac, temp1, temp2);

    ierrSync = cudaGetLastError();
    ierrAsync = cudaDeviceSynchronize(); // Wait for the GPU to finish
    if (ierrSync != cudaSuccess) { printf("Sync error: %s\n", cudaGetErrorString(ierrSync)); }
    if (ierrAsync != cudaSuccess) { printf("Async error: %s\n", cudaGetErrorString(ierrAsync)); }

    // swap the temperature pointers
    temp_tmp = temp1;
    temp1 = temp2;
    temp2= temp_tmp;
  }

  float maxError = 0;
  // Output should always be stored in the temp1 and temp1_ref at this point
  for( int i = 0; i < ni*nj; ++i ) {
    if (abs(temp1[i]-temp1_ref[i]) > maxError) { maxError = abs(temp1[i]-temp1_ref[i]); }
  }

  // Check and see if our maxError is greater than an error bound
  if (maxError > 0.0005f)
    printf("Problem! The Max Error of %.5f is NOT within acceptable bounds.\n", maxError);
  else
    printf("The Max Error of %.5f is within acceptable bounds.\n", maxError);

  free( temp1_ref );
  free( temp2_ref );
  cudaFree( temp1 );
  cudaFree( temp2 );

  return 0;
}
```

