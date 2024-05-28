---
title: 文献阅读方法总结
post_status: publish
skip_file: yes
post_date: 2024-mm-dd hh:mm:ss
---



# Python code block

### 将当前目录设为工作目录并返回

```python
def cwd_chdir():
    import sys,os
    cwd=str(sys.argv[0])
    cwd,pyname=os.path.split(cwd)
    os.chdir(cwd)
    return cwd
```

### 切换到指定目录

```python
#%% work directory & file out
def mkdir(path):
    path=path.strip()
    path=path.rstrip('\')
    isExists=os.path.exists(path)
    if not isExists:
        print (path+' Creats successfully')
        os.makedirs(path)
        return True
    else:
        print (path+' Already exists')
        return False
                     
mkpath='E:\Compressibility\pressure\' + str(' ')
mkdir(mkpath)
os.chdir(mkpath)
```

### 返回cwd目录下符合要求的文件名列表

```python
def filenames(cwd):
    # Creat a filenames list for all test data.
    filenamels = os.listdir(cwd)
    fcsv=[]; fp = []; fm = [];
    #filenamels=[x for x in filenamels if ('.py' not in x)]
    fcsv = [x for x in filenamels if ('.csv' in x)]
    fp = [x for x in filenamels if ('COM' in x)]
    fm = [x for x in filenamels if ('mass' in x)]
    fcsv.sort(); fp.sort(); fm.sort();
    return fcsv, fp, fm
```

### 读取text文件并返回符合要求的数组/np.array快速筛选

```python
def rtxt(ntxt, cond = 0):
    '''
    ntxt: name of the txt file; cond: conditional criterion, which is 0 by default.
    '''
    ftxt = pd.read_table(ntxt, sep=' ')
    ftxt2 = np.array(ftxt.iloc[:,1])
    ftxt2_cond=ftxt2 > cond
    ftxt_ef = ftxt2[ftxt2_cond]
    return ftxt, ftxt_ef
```

### 读取csv文件

```python
# Read
test = np.loadtxt('', delimiter=',')
# Write
np.savetxt('', porosity_map, delimiter=',')
```

### Numpy 多重列表存为npz

```python
# save the csv files into a npz file
surPoints = [surPoints[layer] for layer in surPoints]
np.savez(yarn, *surPoints)
```

### 多个npy存为npz

Note that multiple `npy` files will be generated and they will be compressed and stored in a `.npz` file in the name of `yarn_[yarn number].npz` using the following code:

```python
import numpy as np
from polykriging.utility import cwd_chdir, filenames, zipFilelist


path = "C:/BinY/DMT/Code/processedData/Vf60/warp/"
cwd = cwd_chdir(path)

for i in range(1,2):
    fileList = filenames(cwd, "yarn_" + str(i))

    zipFilelist('./', fileList, "yarn_" + str(i) + ".npz")
```



## 绘图

[Advanced plotting — Python4Astronomers 2.0 documentation](https://python4astronomers.github.io/plotting/advanced.html)

### Area under the curve

```python
import numpy as np
from sklearn.metrics import auc
xx=[]
for i in range(3):
    xx.append(i)
# xx = [.1, .2, .3]
yy = [4, 7, 8]
zz = [4, 7.1, 8.2]
t=auc(xx,yy)
# print('computed AUC using sklearn.metrics.auc: {}'.format(t))
# print('computed AUC using sklearn.metrics.auc: {}'.format(auc(xx,zz)))
```

### Numpy曲线求导

```python
import numpy as np
import matplotlib.pyplot as plt
X = np.arange(0,np.pi2,np.pi2/100)
Y = np.sin(X)
slope_Y = np.diff(Y)/np.diff(X)
plt.plot(X,Y)
plt.plot(X[:-1],slope_Y)
```

### Font and size

```python
    font = {'family' : 'normal',
        'weight' : 'normal', 'size'   : 13}
    plt.rc('font', **font)
    plt.rcParams["font.family"] = "Times New Roman"
```

### Single & multiple plots

```python
# Single image
import matplotlib as plt
fig = plt.figure()
ax = fig.add_axes([0., 0., 1., 1., ])

# multiple subplots
fig1 = plt.figure()
fig2 = plt.figure()
ax1 = fig1.add_subplot(1, 1, 1)
ax2 = fig2.add_subplot(2, 1, 1)
ax3 = fig2.add_subplot(2, 1, 2)

```

### live graph - updating graph

```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from matplotlib import style
style.use( ' fivethirtyeight ' )
fig = plt.figure()
axl = fig.add_subplot(1,1,1)
def animate (i):
    graph_data = open ( ' example.txt ' , ' r ' ) . read()
    lines = graph_data.split( '\n ' )
    xs = []
    ys = []
    for line in lines :
        if len (line) > 1 :
            x , y = line.split ( ',')
            xs.append(x)
            ys.append(y)
    ax1.clear()
    axl .plot()
    
ani = animation.FuncAnimation(fig, animate, interval=500)
plt.show()
```

<img src="CodeBlock.assets/image-20220506172300506.png" alt="image-20220506172300506" style="zoom:67%;" />

### 曲线取极值

```python
def func_extrema(x_arr, N, extrema=[],):
     '''
     x_arr: a 1d ndarray data     
     '''
     from scipy.signal import argrelextrema
     import numpy as np
     
     weights=np.hanning(N)   # data smoothing by The Hanning window
     x_arr_sm = np.convolve(weights/weights.sum(),x_arr)[N-1:-N+1]
     
     hslice = int((N-1)/2)
     
     err_smooth = abs((x_arr_sm - x_arr[hslice:-hslice])).sum()/x_arr.sum()
     
     # for local maxima
     max_ind = argrelextrema(x_arr_sm, np.greater_equal)
     # for local minima
     min_ind = argrelextrema(x_arr_sm, np.less_equal)
     extrema_ind = np.hstack((max_ind, min_ind))
     # include end points
     extrema_ind = np.insert(extrema_ind,0,[0],axis=1)
     extrema_ind = np.insert(extrema_ind,-1,[len(x_arr_sm)-1],axis=1)
     # avoid repeat
     extrema_ind = np.unique(extrema_ind)
     extrema_ind.sort()   # sort by ascending
     return extrema_ind, err_smooth
```

双`Y`坐标或双`X`坐标

```

```





## Image operations

### Image --> Numpy array --> Image

```python
import cv2, os

path = "./v60_704_fill/"

def filenames(cwd):
    # Creat a filenames list for all test data.
    filenamels = os.listdir(cwd)
    ftif=[]
    ftif = [x for x in filenamels if ('.tif' in x)]
    ftif.sort()
    return ftif

ftif = filenames(path)

for image in ftif:
    im = cv2.imread(path + image, 0)  # 0 for cv2.IMREAD_GRAYSCALE
    # im = cv2.imread("r20000.tif", cv2.IMREAD_GRAYSCALE)

    # Otsu's thresholding
    # Get the threshold value tOtsu
    tOtsu,th = cv2.threshold(im,0,255,cv2.THRESH_BINARY+cv2.THRESH_OTSU)  

    _, thim = cv2.threshold(im, tOtsu-5, 100, type=3)

    cv2.imwrite("./v60_704_fill_th/" + image, thim)

```

> ```
> 
> cv2.threshold(	InputArray 		src,      # 源图片，必须是单通道
>                 double 			thresh,	  # 阈值，取值范围0～255
>                 double 			maxval,   # 填充色，取值范围0～255
>                 int 			type 	  # 阈值类型
> 			  )		
> Python:
> cv.threshold(	src, thresh, maxval, type[, dst]	) ->	retval, dst
> 
> ```
>
> | 阈值  | 小于阈值的像素点 | 大于阈值的像素点 |
> | ----- | ---------------- | ---------------- |
> | 0     | 置0              | 置填充色         |
> | 1     | 置填充色         | 置0              |
> | 2     | 保持原色         | 置灰色           |
> | **3** | **置0**          | **保持原色**     |
> | 4     | 保持原色         | 置0              |
>
> ```python
> # 读取待处理图片，并转换成单通道图片
> import cv2
> img = cv2.imread(origin_pic)
> imgray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
> 
> # 对 cv2.threshold 的 type 参数进行 0~6 的实验
> import numpy as np
> for type in range(0, 5, 1):
>     _, thresh = cv2.threshold(src=imgray, thresh=150, maxval=100, type=type)
>     concat_pic = np.concatenate([imgray, thresh], axis=1)
>     cv2.imwrite('./generated_pics/type={}.jpg'.format(type), concat_pic)
> ```

### Numpy array to Image

```python
# 2D
# save a Numpy array to a image file.
from skimage import io
import numpy as np

im = np.zeros(10000)
for i in np.arange(10000):
    im[i]= i%20
im.resize([100,100])
io.imsave('checkboard.jpeg',im,quality = 100)
```

```python
# 3D image sequence
im = np.zeros((100,100,100))
for i in np.arange(100):
    im[i,:,:]= i%20
    #im.resize([100,100])
for i in np.arange(100):
io.imsave('./222/checkboard_'+str(i)+'.jpeg',im[:,:,i].T,quality = 100)   # rotate and save
from scipy.ndimage import rotate
x = np.arange(25).reshape(5, -1)
rotate(x, angle=45, reshape=False)
```

### Numpy array 排序

```python
# sort by x (horizontal coordinate)
sortIndex = centerline[:,0].argsort()
centerlineOrdered = centerline[sortIndex]
```

## Jupyter notebook

### Change the working directory

- Use the jupyter notebook config file:
- Open cmd (or Anaconda Prompt) and run jupyter notebook --generate-config.
- This writes a file to C:\Users\username.jupyter\jupyter_notebook_config.py.
- Browse to the file location and open it in an Editor
- Search for the following line in the file: #c.NotebookApp.notebook_dir = ''
- Replace by c.NotebookApp.notebook_dir = '/the/path/to/home/folder/'
- Make sure you use forward slashes in your path and use /home/user/ instead of ~/ for your home directory, backslashes could be used if placed in double quotes even if folder name contains spaces as such : "D:\yourUserName\Any Folder\More Folders"
- Remove the # at the beginning of the line to allow the line to execute