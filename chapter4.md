# 可用Point类型

为了涵盖能想到的所有可能的情况，PCL中定义了大量的point类型。下面是一小段，在point_types.hpp中有完整目录，这个列表很重要，因为在定义你自己的类型之前，需要了解已有的类型，如果你需要的类型，已经存在于PCL，那么就不需要重复定义了。

* **PointXYZ–成员变量: float x, y, z;**

PointXYZ是使用最常见的一个点数据类型，因为它只包含三维xyz坐标信息，这三个浮点数附加一个浮点数来满足存储对齐，用户可利用points[i].data[0]，或者points[i].x访问点的x坐标值。

```
union
{
    float data[4];
    struct
    {
        float x;
        float y;
        float z;
    };
};
```

* **PointXYZI–成员变量: float x, y, z, intensity;**

PointXYZI是一个简单的XYZ坐标加intensity的point类型，理想情况下，这四个变量将新建单独一个结构体，并且满足存储对齐，然而，由于point的大部分操作会把data[4]元素设置成0或1（用于变换），不能让intensity与xyz在同一个结构体中，如果这样的话其内容将会被覆盖。例如，两个点的点积会把他们的第四个元素设置成0，否则该点积没有意义，等等。因此，对于兼容存储对齐，用三个额外的浮点数来填补intensity，这样在存储方面效率较低，但是符合存储对齐要求，运行效率较高。

```
union
{
    float data[4];
    struct
    {
        float x;
        float y;
        float z;
    };
};
union
{
    struct
    {
        float intensity;
    };
    float data_c[4];
};
```

* **PointXYZRGBA–成员变量: float x, y, z; uint32_t rgba;**

除了rgba信息被包含在一个整型变量中，其它的和PointXYZI类似。

```
union
{
    float data[4];
    struct
    {
        float x;
        float y;
        float z;
    };
};
union
{
    struct
    {
        uint32_t rgba;
    };
    float data_c[4];
};

```

* **PointXYZRGB - float x, y, z, rgb;** 

```
union
{
    float data[4];
    struct
    {
        float x;
        float y;
        float z;
    };
};
union
{
    struct
    {
        float rgb;
    };
    float data_c[4];
};
```

* **PointXY-float x, y;**

简单的二维x-y point结构

```
struct
{
    float x;
    float y;
};
```

