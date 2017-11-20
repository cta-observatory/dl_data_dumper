

```python
import h5py
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
f = h5py.File("gamma_test.hdf5", "r")
```


```python
for items in f.attrs.items():
    print(items)
```

    ('particleType', 0)
    ('converter_version', '1.1')
    ('HDF5_version', '1.8.15')
    ('h5py_version', '2.5.0')


# File structure


```python
for groups in f:
    print(groups)
```

    Cameras
    eventSimu
    showerSimu
    simtel_files
    telescopeInfos


![](hdf5_structure.png)

# Calibrated images


```python
LSTCam_images = f['Cameras/LSTCAM/images']
LSTCam_pixels_pos = f['Cameras/LSTCAM/pixelsPosition']
LSTCam_eventId = f['Cameras/LSTCAM/eventId']
X = LSTCam_pixels_pos[:,0]
Y = LSTCam_pixels_pos[:,1]
```


```python
for i in LSTCam_images:
    plt.scatter(X, Y, c=i,s=9)
    plt.axis('square')
    plt.colorbar()
    plt.show()
```


![png](load_images_files/load_images_8_0.png)



![png](load_images_files/load_images_8_1.png)



![png](load_images_files/load_images_8_2.png)



![png](load_images_files/load_images_8_3.png)



![png](load_images_files/load_images_8_4.png)



![png](load_images_files/load_images_8_5.png)



![png](load_images_files/load_images_8_6.png)



![png](load_images_files/load_images_8_7.png)



![png](load_images_files/load_images_8_8.png)


# Use injunction table to get a square matrix


```python
def camera_to_matrix(image, injTable,nRow,nCol):
    mat = np.zeros((nRow,nCol))
    for i in range(len(X)):
        index = int(injTable[i])
        indexRow = int(index/nCol)
        indexCol = index - int(indexRow*nCol)
        mat[indexRow][indexCol] = image[i]
    return mat
```


```python
injTable = np.array(f['/Cameras/LSTCAM/injTable'])
nbRow = f['/Cameras/LSTCAM'].attrs['nbRow']
nbCol = f['/Cameras/LSTCAM'].attrs['nbCol']
mat = camera_to_matrix(LSTCam_images[-2], injTable, nbRow, nbCol)
```


```python
fig, axs = plt.subplots(ncols=2, figsize=(10, 4))
axs[0].scatter(X, Y, c=LSTCam_images[-2],s=9)
axs[0].axis('equal')
cf = axs[1].imshow(mat)
fig.colorbar(cf, ax=axs[1])
```




    <matplotlib.colorbar.Colorbar at 0x11ddcecc0>




![png](load_images_files/load_images_12_1.png)



```python

```
