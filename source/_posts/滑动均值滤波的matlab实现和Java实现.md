---
title: 滑动均值滤波
date: 2016-07-28 18:38:49
tags: [算法]
---

  最近时间在研究滤波算法，目的是为了更好的识别音频数据。因为有些音频数据里面的杂波太多，很难识别，所以需要先对其进行过滤，才能解析识别。为此，我先在matlab上做了仿真.采用的很多滤波算法，最后选择了对我这个效果最好的，滑动均值滤波。<!-- more -->
  
## 什么是滑动均值滤波 ##

滑动平均滤波就是把连续取得的N个采样值看成一个队列，队列的长度固定为N，每次采样得到一个新数据放到队尾，并丢掉原来队首的一次数据，把队列中的N个数据进行平均运算，就可以获得新的滤波结果
## 具体的matlab代码
```matlab
clear
clc
load boxinfo.mat  %载入音频数据
T = data;
figure(1)
plot(T,'-*')
title('原始数据')
hold on;
%% 
%滑动平滑滤波
L = length(T);
N=10;  % 窗口大下
k = 0;
m =0 ;
for i = 1:L
    m = m+1;
    if i+N-1 > L
        break
    else
        for j = i:N+i-1
            k = k+1;
            W(k) = T(j) ;
        end
        T1(m) = mean(W);
        k = 0;
    end
end
plot(T1,'r-o')
grid
legend('原始数据','滤波之后')

```
### 滤波前后对比图
![滤波前后对比图](http://i.imgur.com/foQOEFL.jpg)
#### 简单分析一下
  经过滑动滤波之后，波形整体变得平滑，这里我们重点关注一下x轴附近的点，可以发现，在波形与x轴交叉的地方，波形都平稳过度，这极大方便的我们后期进行统计。
### 窗口大小选择
从代码中我们可以发现窗口大小我们选择的是10，如何选择窗口大小，这里我们需要进行一些简单的分析和测试。如果x轴附近的噪点数量（一上一下）比较多，那么窗口大小就应该大一些，反之，小一些。但是过大又会出现过拟合的现象，所以可以多取几个值，然后对比一下，选择一个最好的即可。
### 不同的窗口大小对比图
![不同的窗口大小对比图](http://i.imgur.com/SCRASS6.jpg)
### 简单分析一下
从图中我们可以很明显的看出，当N=4的时候，滤波效果还不是很好，在x轴附近依然有噪点（一上一下），当N=7的时候，已经基本满足我们的要求，图形已经可以很平稳的过度了，但是从右边的标记处可以看出还是不是很平稳，所以可以继续提高N值，当N=10的时候，波形就完全能够达到我们的要求，所以取10即可。

## 滑动滤波的Java实现
```Java

/*
 * 功能 对音频数据进行滑动滤波，使其更好的识别  时间：2015/9/11
 */
public class MovingAverageFilter implements IAudioSignalFilter {
    private static final int WINDOWS = 1;
    private short[] mTemp = null; // 只声明暂时不初始化,用来记录最后得不到均值处理的点
    private short[] mBufout = null;
    private int mWindowSize = WINDOWS;

    public MovingAverageFilter(int size) {
        mWindowSize = size;
    }

    // 均值滤波方法，输入一个buf数组，返回一个buf1数组，两者下表不一样，所以定义不同的下表，buf的下表为i，buf1的下表为buf1Sub.
    // 同理，临时的winArray数组下表为winArraySub
    public short[] movingAverageFilter(short[] buf) {
        int bufoutSub = 0;
        int winArraySub = 0;
        short[] winArray = new short[mWindowSize];

        if (mTemp == null) {
            mBufout = new short[buf.length - mWindowSize + 1];
            for (int i = 0; i < buf.length; i++) {
                if ((i + mWindowSize) > buf.length) {
                    break;
                } else {
                    for (int j = i; j < (mWindowSize + i); j++) {
                        winArray[winArraySub] = buf[j];
                        winArraySub = winArraySub + 1;
                    }

                    mBufout[bufoutSub] = mean(winArray);
                    bufoutSub = bufoutSub + 1;
                    winArraySub = 0;
                }
            }
            mTemp = new short[mWindowSize - 1];
            System.arraycopy(buf, buf.length - mWindowSize + 1, mTemp, 0,
                    mWindowSize - 1);
            return mBufout;
        } else {
            short[] bufadd = new short[buf.length + mTemp.length];
            mBufout = new short[bufadd.length - mWindowSize + 1];
            System.arraycopy(mTemp, 0, bufadd, 0, mTemp.length);
            System.arraycopy(buf, 0, bufadd, mTemp.length, buf.length); // 将temp和buf拼接到一块
            for (int i = 0; i < bufadd.length; i++) {
                if ((i + mWindowSize) > bufadd.length)
                    break;
                else {
                    for (int j = i; j < (mWindowSize + i); j++) {
                        winArray[winArraySub] = bufadd[j];
                        winArraySub = winArraySub + 1;
                    }
                    mBufout[bufoutSub] = mean(winArray);
                    bufoutSub = bufoutSub + 1;
                    winArraySub = 0;
                    System.arraycopy(bufadd, bufadd.length - mWindowSize + 1,
                            mTemp, 0, mWindowSize - 1);
                }
            }
            return mBufout;
        }
    }

    public short mean(short[] array) {
        long sum = 0;
        for (int i = 0; i < array.length; i++) {
            sum += array[i];
        }
        return (short) (sum / array.length);
    }
}
```

