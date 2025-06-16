---
title: RGB true-color satellite cloud image
date: 2023-05-03
tags: as
---
Satellite meteorology course homework, teaching you how to draw a satellite cloud image from scratch.
卫星气象课程小作业，教你零基础画卫星云图。
<!--more-->




## 卫星气象课程小作业
原来随便注册一个账号，就可以画卫星图了。。。
下面是一些指导，你需要：
1.网站账号，并完善相关信息
2.matlab
3.就这么多
介绍性的不感兴趣就直接跳过吧

## Satellite Meteorology Course Assignment
It used to be that you could register an account and easily draw satellite images...
Here are some instructions that you need:

1.Website account and complete relevant information
2.Matlab
3.That's it
If you're not interested in the introduction, just skip it.

## English version
#### How to download data:
sign in and follow the link:
>https://ladsweb.modaps.eosdis.nasa.gov/archive/allData/6/MOD021KM


#### Data Introduction:
The MODIS Level-1B dataset contains radiance values of 36 discrete spectral bands ranging from 0.4 to 14.4 micrometers, which have been calibrated and geolocated. Channels 1 and 2 have a resolution of 250 meters, channels 3 to 7 have a resolution of 500 meters, and the rest have a resolution of 1 kilometer. It should be noted that the radiance data and associated uncertainties of the 250 and 500-meter bands have been aggregated to a 1-kilometer resolution.

A single MODIS 1B data file contains one scene established by 203 scans, sampled 1354 times in the cross-track direction, equivalent to approximately 5 minutes of data. The scene is composed of 1354 by 2030 pixels and covers a spatial range of 2330 kilometers by 2030 kilometers.

MOD021KM and MYD021KM are the datasets for the Terra and Aqua satellites, respectively.
Analyzing the link above, it is not difficult to find that it points to the data of the Terra satellite, and users can choose the time according to their needs. The data selected in the Matlab code below is MOD021KM.A2019173.0125.061.2019176162129.hdf.

#### Basic Principle:
True-color composite refers to the composite color processing of multispectral remote sensing images. If the wavelengths of the three bands involved in the composite are the same or similar to the wavelengths of the corresponding red, green, and blue primaries, the color of the composite image will approximate the true color of the ground objects.

#### Basic Method:

Read the reflectance data of 0.46mm, 0.55mm, and 0.64mm of 1km resolution in the MODIS Level-1B dataset.
Combine them as red, green, and blue primaries respectively (R = 0.64mm reflectance, G = 0.55mm reflectance, and B = 0.46mm reflectance).
Use the three-dimensional vector obtained from step 2) to draw an RGB true-color image.
## 中文版本
#### 如何得到数据:
注册一个账号，并进入以下网址:
>https://ladsweb.modaps.eosdis.nasa.gov/archive/allData/6/MOD021KM

#### 数据简介
MODIS Level-1B数据集包含位于0.4微米至14.4微米光谱区域的36个离散波段的辐射值，并进行了校准和地理定位。其中，通道1和2的分辨率为250米，通道3至7的分辨率为500米，其余为1公里的分辨率。值得注意的是，250米和500米波段的辐射度数据及其相关的不确定性已经汇总到1公里分辨率。
一个单一的MODIS 1B级数据文件将包含一个由203次扫描建立的场景，在交叉轨道方向上采样1354次，相当于大约5分钟的数据。场景将由1354乘以2030像素组成，空间覆盖范围为2330公里乘2030公里。
MOD021KM、MYD021KM分别是Terra卫星和Aqua卫星的数据集。
分析上面的链接不难发现其指向的是Terra卫星的数据，大家可以根据需求选着时间。在下面的matlab代码中选择的数据是>MOD021KM.A2019173.0125.061.2019176162129.hdf

#### 基本原理
真彩色合成是指多光谱遥感图像彩色合成处理时，如果参与合成的三个波段的波长与对应的红、绿、蓝三种原色的波长相同或近似，那么合成图像的颜色就会近似于地面景物的真实颜色的一种合成。
#### 基本方法
1）读取MODIS Level-1B数据集中，1km分辨率的0.46mm、0.55mm、0.64mm的反射率数据；
2）分别用0.64mm、0.55mm、0.46mm作为红绿蓝三原色进行组合
（R = 0.64mm reflectance, G = 0.55mm reflectance, and B = 0.46mm reflectance）；
3）利用2）组合成的三维向量绘制RGB真彩色图像。
## 相关代码
## matlab:
```
filename='MOD021KM.A2019173.0125.061.2019176162129.hdf';
datdinfo=hdfinfo(filename);
EV_250_Aggr1km_RefSB = hdfread(filename, 'EV_250_Aggr1km_RefSB');
EV_500_Aggr1km_RefSB = hdfread(filename, 'EV_500_Aggr1km_RefSB');
EV_1KM_Emissive = hdfread(filename, 'EV_1KM_Emissive');
ref_064=EV_250_Aggr1km_RefSB(1,:,:);
ref_064=squeeze(ref_064);
ref_064=double(ref_064);
info250mv = {datdinfo.Vgroup.Vgroup(2).SDS(5).Attributes.Value};
reflectance_scales_250m = double(cell2mat(info250mv(9)));
reflectance_offsets_250m = double(cell2mat(info250mv(10)));
ref_064=reflectance_scales_250m(1)*(ref_064-reflectance_offsets_250m(1));
ref_046=EV_500_Aggr1km_RefSB(1,:,:);
ref_046=squeeze(ref_046);
ref_046=double(ref_046);
info500mv = {datdinfo.Vgroup.Vgroup(2).SDS(8).Attributes.Value};
reflectance_scales_500m = double(cell2mat(info500mv(9)));
reflectance_offsets_500m = double(cell2mat(info500mv(10)));
ref_046=reflectance_scales_500m(1)*(ref_046-reflectance_offsets_500m(1));
ref_055=EV_500_Aggr1km_RefSB(2,:,:);
ref_055=squeeze(ref_055);
ref_055=double(ref_055);
ref_055=reflectance_scales_500m(2)*(ref_055-reflectance_offsets_500m(2));
lat = hdfread(filename,'Latitude');
lon = hdfread(filename,'Longitude');
[row, col]= size(ref_055);
lon1 = imresize(lon,[row, col]);
lat1 = imresize(lat,[row, col]);
lon1=double(lon1);
lat1=double(lat1);
latmin=min(min(lat1));
lonmin=min(min(lon1));
latmax=max(max(lat1));
lonmax=max(max(lon1));
ref_064(ref_064>1)=1;
ref_064(ref_064<0)=0;
ref_064=ref_064.^(0.4);
ref_055(ref_055>1)=1;
ref_055(ref_055<0)=0;
ref_055=ref_055.^(0.4);
ref_046(ref_046>1)=1;
ref_046(ref_046<0)=0;
ref_046=ref_046.^(0.4);
rgb_modis(:,:,1)=ref_064; %Red
rgb_modis(:,:,2)=ref_055; %green
rgb_modis(:,:,3)=ref_046; %blue
hsv_modis=rgb2hsv(rgb_modis);
hsv_modis(:,:,2)=hsv_modis(:,:,2)*2;
rgb_modis=hsv2rgb(hsv_modis);
figure
h=axesm ('Frame','on','MapProjection','eqdcylin','FLineWidth',3, 'Grid',...
    'on','GColor','k','GLineWidth',1, 'GLineStyle', '--',...
    'PLineLocation',5,'MLineLocation',5,'ParallelLabel','on',...
    'PLabelLocation', 10,'MLabelLocation', 10,'MeridianLabel','on',...
     'MapLatLimit',[latmin latmax],'MapLonLimit',[lonmin lonmax],...
     'FontName','Times New Roman','FontSize',12,...
     'FontWeight','normal','LabelUnits','degrees');
geoshow(lat, lon, rgb_modis);
box off
set(gca,'Visible','off')
print(gcf,'-r300','-dpng',['MOD021KM.A2019173.0125_truecolorRGB.png']);
```
### Explanation of Code
Here are some commands that may be useful:

To read detailed information of the hdf file: hdfinfo
To read data from hdf file: hdfread
To convert array to double precision: double
To resize an array: imresize
To draw an image with a map projection: axesm + geoshow

For specific function usage, please refer to the help documentation of Matlab.

Here are some commands for enhancing the image:

When using 0.64mm, 0.55mm, and 0.46mm as the red, green, and blue primary colors for combination, the data for each channel can be adjusted as follows:
Limit the data range between 0 and 1;
Use the following formula for adjustment: data = data^a (a ~ 0.4);

Before drawing the image, use the rgb2hsv function to convert the combined RGB array to the HSV array. After enhancing the saturation, use the hsv2rgb function to convert back to the RGB array (enhancement coefficient b ~ 2).
The values of a and b are not fixed and can be experimented with within a reasonable range.


### 代码相关说明
一些可能需要的命令：
读取hdf文件的详细信息： hdfinfo
读取hdf文件中的数据：     hdfread
将数组转换为双精度：       double
调整数组大小：                   imresize
绘制地图投影的图像：    axesm + geoshow
具体的函数使用，参考matlab的help文档

一些用于美化图像的命令：
1）在利用0.64mm、0.55mm、0.46mm作为红绿蓝三原色进行组合时，对每一个通道的数据可以进行如下修正：
限定数据范围上下限为0和1；
可用如下公式进行修正：data = data^a
(a~0.4）；
2）绘图前，利用rgb2hsv函数将组合成的RGB数组转化为HSV数组，增强饱和度之后，再利用hsv2rgb函数转化回RGB数组（增强系数b ~2）。
a，b没有固定值，可以在合理范围内尝试。

![](1.png)
