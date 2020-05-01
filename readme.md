image only work with py2
requirements--image & numpy


from PIL import Image
import numpy as np

# 图像识别
a=np.array(Image.open("C:/Users/花花/Desktop/image/fj.jpg").convert('L')).astype('float')

# 梯度重构和归一化
depth=10.                            # 预设深度值为10，取值范围（0-100）
grad=np.gradient(a)                  # 取图像灰度的梯度值
grad_x,grad_y=grad                   # 分别取横纵轴方向梯度值
grad_x=grad_x*depth/100              # 根据深度调整横轴和纵轴方向的梯度值
grad_y=grad_y*depth/100
A=np.sqrt(grad_x**2+grad_y**2+1)     # 构建三维归一化单位坐标系
uni_x=grad_x/A
uni_y=grad_y/A
uni_z=1./A

# 光源的调整和归一化
vec_el=np.pi/2.2                      # 预设光源的俯视角度，弧度值
vec_az=np.pi/4.                       # 预设光源的方位角度，弧度值
dx=np.cos(vec_el)*np.cos(vec_az)      # 光源对X轴的影响
dy=np.cos(vec_el)*np.sin(vec_az)      # 光源对y轴的影响
dz=np.sin(vec_el)                     # 光源对z轴的影响

b=255*(dx*uni_x+dy*uni_y+dz*uni_z)    # 梯度与光源相互作用，实现灰度值重构
b=b.clip(0,255)                       # 避免数据越界，将生成的灰度裁剪成0-255区间

im=Image.fromarray(b.astype('uint8'))    #重构为图像类型
im.save("C:/Users/花花/Desktop/image/fjHD.jpg")